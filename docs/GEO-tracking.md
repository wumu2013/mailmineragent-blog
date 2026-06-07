# GEO 效果追踪表

> 监控目标页：`/posts/keyboard-riser-niche-tiktok-hustle/`
> 优化落地日期：2026-06-08
> 跟踪周期：每周一次，连续 4 周，之后转月度复盘

## 1. AI 平台引用监测

| 平台 | 测试问题 | 是否引用 | 引用片段 | 引用日期 | 备注 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Google AI Overviews | "Can you sell a custom keyboard riser for long nails on TikTok Shop?" |  |  |  |  |
| Google AI Overviews | "How to find a niche product for TikTok Shop with no ad spend?" |  |  |  |  |
| Perplexity | "keyboard riser niche TikTok shop case study" |  |  |  |  |
| Perplexity | "tiktok shop 30 orders per day zero ad spend playbook" |  |  |  |  |
| Gemini | "How does the niche-within-a-niche TikTok strategy work?" |  |  |  |  |
| Gemini | "Custom keyboard riser for typing with long nails sourcing" |  |  |  |  |
| ChatGPT | "Best niche products for TikTok Shop beginners 2026" |  |  |  |  |
| ChatGPT | "How much profit can a single TikTok Shop niche product make?" |  |  |  |  |

测试方法：在每个平台输入问题后，记录 (a) 是否在答案中看到 mailmineragent.com 域名，(b) 引用片段的原文，(c) 测试日期。

## 2. Google Search Console 监控

| 指标 | 优化前基线 | 第 1 周 | 第 2 周 | 第 4 周 | 第 8 周 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 索引状态 | □ 已索引 / □ 未索引 |  |  |  |  |
| 抓取频率（次/周） |  |  |  |  |  |
| 关键词排名（"keyboard riser niche"） |  |  |  |  |  |
| 关键词排名（"tiktok shop case study 30 orders"） |  |  |  |  |  |
| 关键词排名（"niche within a niche tiktok"） |  |  |  |  |  |
| 自然点击数 |  |  |  |  |  |
| AI Overview 触发次数 |  |  |  |  |  |

## 3. 基础设施验证清单

- [x] `public/llms.txt` 已生成且可访问
- [x] 目标页 `<head>` 包含 FAQPage JSON-LD
- [x] 博客 `Organization` Schema 通过 `[params.schema]` 配置生效
- [x] 关键数据加粗（30–50 orders、$4K–$8K、$15B 指甲油市场）
- [x] H1 改为疑问句
- [x] FAQ 6 问 6 答正文段落
- [ ] 提交 sitemap 至 GSC（若未提交）
- [ ] 验证 Rich Results Test 通过（schema.org/FAQPage）

## 4. 迭代记录

### 2026-06-08 — GEO 改造首版落地
- 改动文件 5 个：`content/posts/keyboard-riser-niche-tiktok-hustle.md`、`config.toml`、新建 `layouts/_partials/extend_head.html`、`layouts/_default/home.llms.txt`、`docs/GEO-tracking.md`
- 关键变更：H1 改疑问句 + 6 处关键数据加粗 + FAQ frontmatter 字段 + 正文 FAQ 段落 + Organization Schema + 自动渲染 llms.txt
- 下一步：观察 GSC 抓取频率、提交 sitemap、用 Rich Results Test 验证 FAQPage

### （后续记录）
