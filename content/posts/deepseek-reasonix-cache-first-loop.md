---
title: "DeepSeek-Reasonix: What a Cache-First Architecture Actually Looks Like"
date: 2026-05-29
draft: false
tags: ["LLM", "AgentDevelopment", "CostOptimization", "DeepSeek", "Reasonix", "PromptCaching", "CodeAgent", "Benchmark"]
description: "DeepSeek's prefix cache can hit 94-99% — if your agent is designed for it. Here is the architecture and the real data."
---

## Recap: The Cache Mismatch Problem

In our [previous post]({{< ref "why-claudecode-opencode-deepseek-cache-mismatch.md" >}}), we explained why pairing OpenCode / ClaudeCode with DeepSeek destroys your cache hit rate:

- DeepSeek uses **strict full-prefix matching** — a cache hit only fires when every byte from position `0` is identical to the previous request.
- Agent loops insert `tool` messages in the middle of the message array, breaking the prefix hash every turn.
- Anthropic-style `cache_control` segment markers are silently ignored.
- Result: **near-zero cache hit rate**, even though DeepSeek's billing dashboard shows caching is "enabled."

The problem is not DeepSeek. The problem is that _generic agent frameworks were designed for a fundamentally different caching mechanic_.

---

## What DeepSeek-Reasonix Does Differently

[DeepSeek-Reasonix](https://github.com/esengine/DeepSeek-Reasonix) is a DeepSeek-native coding agent built from the ground up around one invariant: **the byte prefix must never change between turns.**

> Cache stability is not a feature you turn on; it is an invariant the loop is designed around.

### Pillar 1: Cache-First Loop

The context is partitioned into three regions, each with strict mutation rules:

```
┌─────────────────────────────────────────────┐
│ IMMUTABLE PREFIX                             │ ← frozen for the session
│ system prompt + tool specs + few-shots       │   same bytes every turn
├─────────────────────────────────────────────┤
│ APPEND-ONLY LOG                              │ ← grows monotonically
│ [assistant₁][tool₁][assistant₂][tool₂]...    │   never reorders, never edits-in-place
├─────────────────────────────────────────────┤
│ VOLATILE SCRATCH                             │ ← reset each turn
│ R1 reasoning, plan state, ephemeral COT      │   never sent to the API
└─────────────────────────────────────────────┘
```

Four concrete mechanisms enforce this:

1. **`ImmutablePrefix`** — system prompt and tool specifications are serialized once at session start. Same byte sequence every model call. No date strings, no directory paths, no runtime variables injected into the cached region.

2. **`AppendOnlyLog`** — new turns are only ever appended to the end. No message re-ordering, no compaction that rewrites earlier positions. The prefix hash from turn `N` is a valid prefix for turn `N+1`.

3. **`VolatileScratch`** — chain-of-thought, intermediate plans, and per-turn scratch buffers live in a separate region that is never included in the API request. This prevents R1's lengthy `reasoning_content` from polluting the byte prefix of the next turn.

4. **Auto-compaction** — when context approaches the token limit, older turns are folded into a summary message _appended_ to the log. The prefix itself is never rewritten, so the cache survives the compaction.

### Pillar 2: Tool-Call Repair

DeepSeek models have quirks in tool calling — truncated JSON, calls leaked into `reasoning_content`, repeated identical calls. Reasonix applies four repair passes:

- **flatten** — schemas with deep nesting (>2 levels) or wide surface area (>10 params) are auto-detected and presented in dot-notation form; `dispatch()` re-nests before calling your function.
- **scavenge** — regex sweeps `reasoning_content` for tool calls the model forgot to emit in the structured `tool_calls` field.
- **truncation** — detects unbalanced JSON and either closes braces or requests a continuation completion.
- **storm** — identical `(tool, args)` tuples within a sliding window are suppressed; a reflection turn is injected instead.

### Pillar 3: Cost Control

- **Adaptive model selection**: `auto` preset starts on `v4-flash`, escalates to `v4-pro` mid-turn if the model shows signs of struggle (edit failures, repeated repair triggers). Threshold: 3 failure signals per turn.
- **Auxiliary calls use flash**: summarization, subagent spawns, and truncation repair all hard-code `v4-flash` regardless of the user's preset.
- **Turn-end compaction**: tool results exceeding 3000 tokens are shrunk to a summary at turn boundaries. Full text is available via `read_file` if needed.
- **Per-turn cost badges**: the TUI shows a green/yellow/red badge for every turn, so you always know what you are paying.

---

## The Data: Synthetic Benchmark (τ-bench-lite)

Reasonix publishes a controlled τ-bench-lite benchmark: **8 multi-turn tool-use tasks × 3 repeats = 48 runs per side**. Same tools, same prompts, same DeepSeek API client — the sole variable is prefix stability.

| Metric | Baseline (cache-hostile) | Reasonix (CacheFirstLoop) | Delta |
|---|---:|---:|---:|
| Runs | 24 | 24 | — |
| **Cache hit rate** | 46.6% | **94.4%** | **+47.7 pp** |
| Cost / task | $0.002599 | $0.001579 | **−39% (×0.61)** |
| vs Claude Sonnet 4.6 (token-count est.) | — | — | **~96% cheaper** |
| Pass rate | 96% (23/24) | **100% (24/24)** | Reasonix held every guardrail |

A 39% cost reduction purely from byte-stable prefix engineering — same model, same API key, same task set.

The transcripts are committed to the repository with per-turn `usage`, `cost`, and `prefixHash` fields for independent verification. Reasonix's `prefixHash` stays constant across all model calls in a session; baseline's hash changes on every turn.

---

## The Data: Real-World User, Single Day

On **2026-05-01**, a real Reasonix user shared their DeepSeek dashboard (used with permission, anonymized):

| | Tokens |
|---|---:|
| Input — cache hit | 435,033,856 |
| Input — cache miss | 767,616 |
| Output | 179,763 |
| **Day total** | **435,981,235** |

**Cache hit ratio: 99.82%**

At DeepSeek v4-flash pricing ($0.028/Mtok cached, $0.139/Mtok uncached):

| | This user (99.82% hit) | Same workload, 0% cache |
|---|---:|---:|
| Cache-hit input | $12.18 | — |
| Cache-miss input | $0.11 | $60.58 |
| Output | $0.05 | $0.05 |
| **Total / day** | **$12.34** | **$60.63** |

→ **~80% savings** on a single day of heavy use. On v4-pro the saving grows to ~91%.

---

## Comparison: Cache Hit Rates Across Clients

DeepSeek's prefix cache is always on at the API level. The difference is entirely client-side:

| Client | Typical cache hit rate | Why |
|---|---|---|
| DeepSeek web chat | 60–80% | Single conversation, no tool calls — but drops to 0% on new session |
| Cherry Studio / Open WebUI | 30–60% | History reordering, reserialized tool specs |
| Cline / Continue | <30% | XML tool calls inline in conversation, bytes shift every turn |
| OpenCode + DeepSeek | near 0% | `cache_control` markers ignored + middle-message insertion |
| **Reasonix** | **94–99.8%** | Immutable prefix + append-only log + volatile scratch + auto-compact |

> "DeepSeek gave us cacheable bytes. The four mechanisms above are how we keep the bytes cacheable."
> — Reasonix architecture docs

---

## Key Architectural Insight

The difference between Reasonix and a generic agent framework is not incremental — it is a fundamentally different loop design:

| Dimension | Generic framework (OpenCode, LangChain, etc.) | Reasonix |
|---|---|---|
| Cache model | Anthropic `cache_control` segments (or none) | DeepSeek full-prefix |
| System prompt | Rebuilt each turn, may contain dates/paths | Frozen at session start (`ImmutablePrefix`) |
| Message history | Reordered, compacted, edited in-place | Append-only, never mutated |
| Tool results | Inlined into conversation | Appended after tool call; auto-compacted to summary |
| COT / scratch | Included in API request (poisons prefix) | Volatile scratch, never sent upstream |
| Failover model | N/A | Auto-escalation from flash → pro on struggle |

This is not about being "better engineered" in the abstract. It is about **matching the loop structure to the cache mechanic**. Generic frameworks were designed for OpenAI / Anthropic's cache model; Reasonix was designed for DeepSeek's. Neither is wrong — but mixing them wastes your largest cost lever.

---

## Summary

DeepSeek's prefix cache is real and delivers massive savings — **94–99.8% cache hit rates, 39–80% cost reduction** — but only when your agent loop preserves byte-level prefix stability.

DeepSeek-Reasonix achieves this through three pillars:
1. **Cache-First Loop** — immutable prefix, append-only log, volatile scratch, auto-compaction
2. **Tool-Call Repair** — flatten, scavenge, truncation fix, storm suppression
3. **Cost Control** — adaptive model selection, auxiliary route to flash, turn-end compaction

The numbers are published and independently verifiable: the benchmark harness, JSONL transcripts, and replay tool are all MIT-licensed at [github.com/esengine/DeepSeek-Reasonix](https://github.com/esengine/DeepSeek-Reasonix).

If you are running DeepSeek through a generic agent framework and wondering why your cache hit rate hovers near zero, this is why. The cache was never the problem; the loop was.

---

## FAQ

### Q: Can I use Reasonix with non-DeepSeek models?

No — and that is by design. Reasonix is DeepSeek-only because every layer is tuned to DeepSeek's specific properties: byte-stable prefix caching, cheap token pricing, R1 reasoning traces, and JSON mode. Pointing it at OpenAI would lose the cache economics that justify the architecture.

### Q: How does Reasonix compare to Claude Code in practice?

Claude Code costs roughly **10–50× more per task** depending on model tier and caching behavior. Reasonix's τ-bench benchmark shows ~96% cheaper vs Claude Sonnet 4.6 on a token-count basis. The trade-off is that Claude Code supports Anthropic's native `cache_control` and works with a broader ecosystem; Reasonix optimizes ruthlessly for the DeepSeek stack.

### Q: Is 94–99.8% cache hit rate sustainable over weeks?

The mechanisms are designed for long-running sessions: `ImmutablePrefix` never changes within a session, `AppendOnlyLog` never rewrites history, and auto-compaction folds older context without touching the prefix. The real-world 99.82% case covers a full day of active use. Sessions persist per workspace and survive restarts via disk persistence.

### Q: What happens when the context window fills up?

Reasonix auto-compacts: older turns are folded into a summary message that is _appended_ to the log. The immutable prefix is never rewritten, so the cache survives. After compaction, the prefix remains byte-stable — only the appended summary changes.

### Q: Can I get Reasonix's caching benefits in my own agent framework?

The core primitives are MIT-licensed and importable: `CacheFirstLoop`, `ImmutablePrefix`, `AppendOnlyLog`, and `ToolRegistry` are all available as library components via `npm install reasonix`. You can build your own agent on top of the same loop without adopting the full Reasonix CLI.

### Q: Does the auto-escalation to v4-pro cause unexpected costs?

The threshold is conservative (3 failure signals per turn) and signaled via a yellow warning badge in the TUI before escalation happens. The escalation applies only to the _remainder of the current turn_ and resets at every turn start. In practice, most sessions stay entirely on v4-flash.

### Q: What if I do not want to use the TUI?

Reasonix supports non-interactive modes: `npx reasonix run "ask anything"` for one-shot queries, `reasonix code --no-session` for ephemeral sessions, and `reasonix stats / replay / diff` for offline transcript analysis. The TUI is optional.
