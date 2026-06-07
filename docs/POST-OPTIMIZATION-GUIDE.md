# 文章优化指南（Post Optimization Guide）

> 本文档记录 MailMiner Agent Blog 团队对存量文章做 SEO + E-E-A-T 整改时沉淀下来的标准流程。所有阈值、模板、命令均来自对 `keyboard-riser-niche-tiktok-hustle`、`kitchen-supply-wholesale-laos-sichuan-entrepreneur` 与 `from-shenzhen-university-to-laos-clothing-empire` 三篇的实际优化经验。

---

## 📑 目录

- [0. 适用场景与术语](#0-适用场景与术语)
- [1. 模块 A：Front Matter SEO 升级](#1-模块-afront-matter-seo-升级)
- [2. 模块 B：标题层级规范化](#2-模块-b标题层级规范化)
- [3. 模块 C：E-E-A-T 强化](#3-模块-ce-e-a-t-强化)
- [4. 模块 D：封面图与视觉资产](#4-模块-d封面图与视觉资产)
  - [4.1 图片规格](#41-图片规格)
  - [4.2 生成工具：9router / 20128](#42-生成工具9router--20128)
  - [4.3 Prompt 写作公式](#43-prompt-写作公式)
  - [4.4 Alt 文本写作公式](#44-alt-文本写作公式)
  - [4.5 验证 alt 真的进了 `<img>` 标签](#45-验证-alt-真的进了-img-标签)
  - [4.6 封面图的迭代生成（不满意的常见场景）](#46-封面图的迭代生成不满意的常见场景)
- [5. 收尾：语言清理与单位补全](#5-收尾语言清理与单位补全)
- [6. 构建与验证](#6-构建与验证)
- [7. 提交规范](#7-提交规范)
- [8. 常见错误清单](#8-常见错误清单)

---

## 0. 适用场景与术语

### 适用对象

存量 `content/posts/*.md` 文章，存在以下任一症状：

- Google Search Console 显示**已抓取未收录**（crawled, not indexed）
- Title 超过 60 字符
- 无 `keywords` 字段、缺作者背书、缺外链引用
- 全文无 H1、目录章节 H2 缺少长尾词
- 文章内关键数据无来源

### 关键术语

| 术语 | 含义 |
|---|---|
| **E-E-A-T** | Experience / Expertise / Authoritativeness / Trustworthiness，Google 评估内容质量的四大维度 |
| **YMYL** | Your Money or Your Life，金融/健康/商业类内容受 E-E-A-T 影响最大 |
| **TL;DR** | 放在 H1 之下的 2-3 句总结块引，帮助爬虫与读者快速抓取核心 |
| **Long-tail keyword** | 长尾关键词，搜索量低但意图精准，3-5 词短语 |

### 优化前的硬性约束

- ✅ 全文**保持英文**（与博客定位一致；中文残留是缺陷不是优势）
- ✅ **不引入联盟链接、私域引流、付费推广**（新站沙盒期尤其敏感）
- ✅ 不得**删改文章事实数据**（$280K、30-50 orders/day 等数字来自采访对象，**只补充来源，不得改写**）
- ✅ 不修改 `themes/PaperMod`（升级会冲掉改动）
- ✅ 不动 `config.toml` 的 site-level 配置（除非本次任务明确要求）

---

## 1. 模块 A：Front Matter SEO 升级

### 1.1 目标字段

修改文章前 1-7 行的 front matter，目标如下：

| 字段 | 当前状态 | 目标 | 阈值 |
|---|---|---|---|
| `title` | 76-99 字符（**超长**） | 含长尾词、≤60 字符 | 50-60 |
| `description` | 254-510 字符（**超长**） | 含核心词 + 价值引导 | 120-160 |
| `keywords` | **缺失** | 6-8 个长尾词数组 | 6-8 |
| `author` | site default | "MailMiner Editorial Team" | 固定 |
| `cover` | **缺失** | image + alt + caption 块 | 见 §4 |

### 1.2 Title 重写公式

**公式**：`[主关键词] [次关键词]: [价值主张 / 数字]`

**实测案例**：

```yaml
# ❌ 优化前（76 字符，被 Google 截断）
title: "30-50 Orders a Day From a Keyboard Riser: The Niche-Within-a-Niche Playbook"

# ✅ 优化后（60 字符，命中 3 个长尾词）
title: "Keyboard Riser Niche TikTok Hustle: 30 Orders a Day Playbook"
```

```yaml
# ❌ 优化前（99 字符）
title: "A $280K Kitchen Supply Warehouse in Laos: Why This Sichuan Entrepreneur Says Budget 60% Above Your Estimate"

# ✅ 优化后（60 字符）
title: "Kitchen Supply Wholesale in Laos: A $280K Vientiane Playbook"
```

```yaml
# ❌ 优化前（107 字符，含全名"Shenzhen University"和"2000s-Born"——Google 截断)
title: "From Shenzhen University to a Clothing Empire in Laos: A 2000s-Born Entrepreneur's $700K Bet on Southeast Asia"

# ✅ 优化后（54 字符，person-identifier 缩写 + 城市 + 金额 + subject)
title: "Shenzhen Grad Builds $700K Menswear Store in Vientiane"
```

**避坑**：
- 不要把"人名"或"采访对象"放进 Title（搜的人不关心是谁）
- 不要把**全名**的机构名（如 "Shenzhen University"）放 Title——缩写为 "Shenzhen Grad" / "Sichuan Seller" 等 person-identifier，更利于搜索匹配
- 不要用 clickbait 副标（"You won't believe..."）—— Google 降权
- 数字尽量用阿拉伯数字（"$280K" 而非 "two hundred and eighty thousand"）

### 1.3 Description 重写公式

**公式**：`[Who] [did what] [with what result] — [对读者的价值引导]`

**实测案例**：

```yaml
# ❌ 优化前（254 字符，被截断）
description: "A regular employee with zero e-commerce background found a hyper-specific niche — keyboard risers for women with long nail art. By customizing a product for one pain point and posting TikTok content, they built a steady 30-50 orders per day with zero ad spend."

# ✅ 优化后（159 字符，含数字 + 价值 + 关键词）
description: "How a beginner sells custom keyboard risers for women with long nail art on TikTok Shop — 30 orders/day, $4-8K monthly profit, zero ad spend. Niche case study."
```

```yaml
# ❌ 优化前（252 字符，重复 + 长串）
description: "A 2000s-born CS graduate from Shenzhen University bypassed Big Tech and invested $700K to open a menswear store in Laos. After scouting 10 countries across Central Asia, Africa, and Southeast Asia, he built a business doing $1,000+ daily revenue — and claims 5-month payback."

# ✅ 优化后（160 字符，含 5 个核心数字 + 价值引导）
description: "A Shenzhen University CS grad skipped Big Tech to invest $700K in a Vientiane menswear store. $1,000/day revenue, 5-month payback. Cross-border retail playbook."
```

**调试字符数的快速方法**：

```bash
python3 -c "print(len('YOUR DESC HERE'))"
```

### 1.4 keywords 数组

**原则**：
- 6-8 个，**不要堆砌**（超过 10 个 Google 会判定 keyword stuffing）
- 优先**短语而非单词**（`keyboard riser niche` 比 `keyboard` 强 10 倍）
- 必须包含文章核心 + 1-2 个场景变体 + 1 个目标受众词

**实测案例**：

```yaml
keywords: [
  "keyboard riser niche",        # 核心
  "tiktok shop case study",      # 内容类型
  "niche ecommerce playbook",    # 主题域
  "long nail typing solution",   # 痛点变体
  "tiktok organic traffic",      # 流量策略
  "1688 dropshipping",           # 供应链
  "zero ad spend ecommerce",     # 卖点
  "tiktok shop beginner"         # 受众
]
```

### 1.5 author 字段

**固定写法**：

```yaml
author: "MailMiner Editorial Team"
```

**不要写**："Anonymous" / "MailMiner Agent" / "The Team"（Google 的 E-E-A-T 评估器会判定为**不可信**）。

### 1.6 cover 块（可选但强烈推荐）

**结构**：

```yaml
cover:
  image: "images/POST-SLUG-cover.jpg"  # 或 .png，但 .jpg 优先
  alt: "[对象] + [场景] + [关键词]"
  relative: false
  caption: "[简短说明，HTML 内渲染为 figcaption]"
```

**图片生成规范**见 §4。

---

## 2. 模块 B：标题层级规范化

### 2.1 核心原则

1. **全文恰好 1 个 H1**，禁止多 H1
2. PaperMod 的 `single.html` 会**自动**用 front matter `title` 渲染 H1
3. **不要在 markdown 顶部手动写 `# H1`**——这会产生双 H1，导致 SEO 评分下降
4. H1 → H2 → H3 不得跳级

### 2.2 标准结构

```markdown
---
title: "..."           # 渲染为 H1
date: ...
---

> **TL;DR** 2-3 句总结，含核心数据  ← 块引，不是标题

## Section 1: 含长尾词的 H2
### Sub-section 1.1
### Sub-section 1.2

## Section 2: 含长尾词的 H2
...以此类推

---

## About the MailMiner Editorial Team   ← 结尾 E-E-A-T section，详见 §3
```

### 2.3 H2 长尾词改造规则

**不要**做：把 H2 改成纯关键词堆砌（"Kitchen Supply Wholesale Laos Vientiane Sichuan..."）—— Google 判定 spam。

**应该**做：把原 H2 的语义**自然延伸**，把长尾词融入主标题：

| 原 H2 | 优化后 H2 | 命中词 |
|---|---|---|
| `## The 1,200 Square Meter Bet` | `## The 1,200 sqm Warehouse Bet: Pan's Vientiane Store` | `vientiane store` |
| `## The Numbers That Matter` | `## The Numbers: $280K Breakdown of a Vientiane Warehouse` | `vientiane warehouse` |
| `## Finding the Pain Point` | `## Finding the Pain Point: Why Standard Risers Fail` | `standard risers` |
| `## The Customization Pivot` | `## The Customization Pivot: A Taller Keyboard Riser` | `taller keyboard riser` |
| `## The Content Strategy` | `## TikTok Content Strategy for Niche Products` | `tiktok content strategy` |
| `## The Graduate Who Chose Laos Over Big Tech` | `## The Shenzhen Grad Who Chose Vientiane Over Big Tech` | `vientiane` |
| `## Why Domestic E-Commerce Became Unsustainable` | `## Why Chinese Livestream E-Commerce Stopped Working` | `livestream e-commerce` |
| `## The Grand Scouting Tour: 10 Countries, 3 Continents, Half a Year` | `## The 10-Country Scouting Tour Across 3 Regions` | `10 country scouting tour` |
| `## The Laos Bet: Market Structure and Positioning` | `## The Vientiane Menswear Market: A Two-Tier Structure` | `vientiane menswear` |
| `## The Economics: $700K Breakdown` | `## The $700K Investment Breakdown: Capital, Store, Inventory` | `investment breakdown` |
| `## Livestream Commerce, Laos Edition` | `## Livestream Commerce, Vientiane Low-Tech Edition` | `vientiane livestream` |
| `## Beyond the Numbers` | `## Beyond the Numbers: Chinese Entrepreneurial Energy Abroad` | `chinese entrepreneurs` |

### 2.4 TL;DR 块引

**位置**：紧跟 front matter 后，第一个 H2 之前。

**实测案例**：

```markdown
> **TL;DR** A regular employee with zero e-commerce background found a hyper-specific niche — custom keyboard risers for women with long nail art. By modifying one dimension (height) and posting simple TikTok demos, they built a steady 30–50 orders per day with zero ad spend. This case study breaks down the "niche-within-a-niche" playbook that beginners can replicate.
```

**长度**：2-4 句，含 1-2 个核心数字 + 1 个核心方法论。

### 2.5 已知坑：双 H1

如出现 `H1 count: 2`，**先删除 markdown 顶部的手动 H1**（保留 TL;DR），让 PaperMod 自动用 front matter title 渲染 H1。

---

## 3. 模块 C：E-E-A-T 强化

E-E-A-T 是文章能否被收录的**决定性因素**（贡献 80%+ 的判断权重）。

### 3.1 四大短板及对策

| 短板 | 表现 | 整改 |
|---|---|---|
| **作者背书缺失** | 全文只有 "MailMiner Team" | 文末加 3 段 E-E-A-T section（见 §3.2） |
| **来源与佐证不足** | 行业数据无引用 | 至少 1-2 个**高权威外链**（见 §3.3） |
| **导流 / 商业倾向** | 含联盟链接、私域引流 | **删除所有**（新站沙盒期敏感） |
| **内容同质化** | 与同类博客开篇 / 行文雷同 | 增加独家实操细节（供应商名、价格区间、踩坑经历） |

### 3.2 文末 E-E-A-T Section 模板

固定 3 段结构，**复制即用**：

```markdown
---

## About the MailMiner Editorial Team

The MailMiner Editorial Team is a group of cross-border e-commerce operators, TikTok Shop sellers, and AI tooling builders. We publish case studies drawn from real seller interviews and our own product experiments — never generic theory, never fabricated case studies.

**Our focus areas** include TikTok Shop organic commerce, niche product selection and 1688 sourcing, solo-seller $0-ad-spend playbooks, and AI tooling for e-commerce operators. Past coverage includes a [Spanish TikTok-to-Shopify founder's journey](/posts/from-tiktok-to-shopify-spanish-ecommerce/) and the [Amazon refined-selection 90% framework](/posts/amazon-refined-selection-90-percent-success-framework/).

**Disclosure:** Revenue figures ($XXX–$XXX/day, $XK–$XK/month) and operational details (XX–XX orders/day) are reported by the seller, not independently audited. Margin estimates assume a XX–XX% gross margin, typical for this category but variable by supplier and shipping terms.

> **Found a similar niche or have questions about the niche-within-a-niche framework?** Reach out via the [About page](/about/) — we read every message.
```

**三个固定元素**：
1. **Past coverage 内链**：至少 2-3 个 `/posts/...` 链接，构建站内 link juice
2. **Disclosure 数字段**：把文中引用的关键数字做"reported, not audited"声明
3. **CTA 链到 `/about/`**：引流到 about 页形成闭环

### 3.3 外链策略

| 文章类型 | 推荐外链源 |
|---|---|
| 行业市场规模（如 "$15B nail polish market"） | Statista / Grand View Research / IBISWorld |
| 地名/机构（如 Vientiane / National University） | Wikipedia（最稳，几乎零失效风险） |
| 平台（TikTok / 1688 / Amazon） | 官方商家文档 / business blog |
| 通用概念（wholesaling / kitchenware） | Wikipedia |
| 技术（如 model / agent / API） | 官方文档 / GitHub repo / arXiv |

**实测案例**：

```markdown
# Wikipedia（地标/概念，最稳）
[Vientiane](https://en.wikipedia.org/wiki/Vientiane)
[Zigong](https://en.wikipedia.org/wiki/Zigong)
[Wholesaling](https://en.wikipedia.org/wiki/Wholesaling)

# 行业报告（市场规模，准确性需复核）
[Grand View Research's nail polish market report](https://www.grandviewresearch.com/industry-analysis/nail-polish-market-report)

# 平台官方
[TikTok](https://www.tiktok.com/business/en/blog/tiktok-shop-seller)
[1688](https://www.1688.com/)
```

**数量**：5-8 个外链为佳。少于 3 个 E-E-A-T 信号弱，多于 10 个爬虫会判定为 link spam。

### 3.4 内部链接（link juice 闭环）

**最少做这些**：
- ✅ 链到 `/about/`（出现在文末 CTA + E-E-A-T section）
- ✅ 链到 2-3 篇同主题 `/posts/...`（出现在 E-E-A-T section 的 "Past coverage" 段）
- ✅ 文中**自然引用**其它文章时，**主动加超链**（如："the first — a [Shenzhen University graduate's menswear store](/posts/from-shenzhen-university-to-laos-clothing-empire/) with..."）

**系列文章的交叉链接**（series articles cross-link）：

当两篇文章覆盖**同一市场或同一主题**（如中国创业者出海老挝），应在两篇中**互相加 1 条叙述性内链**，形成小型 link graph：

```markdown
# 在 from-shenzhen-university-to-laos-clothing-empire.md 末尾加：
This is the first profile in an informal series on Chinese entrepreneurs in Laos.
The second — a [kitchen supply wholesale warehouse in Vientiane](/posts/kitchen-supply-wholesale-laos-sichuan-entrepreneur/) — captures the _depth_ of the same market: a Zigong entrepreneur who spent $280K and one year of scouting...

# 反向，在 kitchen-supply-wholesale-laos-sichuan-entrepreneur.md 加：
This is the second profile in an informal series on Chinese entrepreneurs in Laos.
The first — [a Shenzhen University graduate's menswear store in Laos](/posts/from-shenzhen-university-to-laos-clothing-empire/) with a five-month payback claim — captured the _velocity_ of Chinese entrepreneurship.
```

**判别标准**：两篇文章满足以下任一条件时，应做 series cross-link：
- 同一地理市场（如都是 Laos / Vientiane）
- 同一受众（如都是"中国出海创业者"）
- 同一商业模型变体（如都是"warehouse store"模式）

> 反向链接**只加 1 条**（不要在 E-E-A-T section 里再列一次，避免重复）—— 叙述性内链 + Past coverage 内链 即可形成 2 条连接，足以让爬虫识别为系列。

---

## 4. 模块 D：封面图与视觉资产

### 4.1 图片规格

| 项 | 推荐值 |
|---|---|
| 格式 | **JPG**（progressive, quality=88） |
| 尺寸 | **1280×720**（16:9，对应 OG image 1200×630 的 16:9 等比放大版） |
| 大小 | 80-200 KB（不要超过 300 KB，影响首屏 LCP） |
| Alt 文本 | 50-125 字符，含核心关键词 |

### 4.2 生成工具：9router / 20128

项目使用 `~/.agents/utils/router_i2i.py` 调用 9router 的 image-01 模型。

**纯文生图调用模板**：

```bash
python3 -c "
import sys
sys.path.insert(0, '/home/wumu/.agents/utils')
from router_i2i import generate_image, DEFAULT_API_KEY, RATIO_TO_SIZE
import os, base64, urllib.request, io
from PIL import Image

result = generate_image(
    prompt='YOUR_ALL_ENGLISH_PROMPT_HERE',
    ref=None,                      # 纯文生图
    ratio='16:9',                  # -> 1280x720
    size=RATIO_TO_SIZE['16:9'],
    api_key=os.environ.get('ROUTER_IMAGE_API_KEY', DEFAULT_API_KEY),
    model='minimax-cn/image-01',
)
url = result['data'][0].get('url') or result['data'][0].get('b64_json', '')
if url.startswith('http'):
    raw = urllib.request.urlopen(url, timeout=60).read()
else:
    raw = base64.b64decode(url)

img = Image.open(io.BytesIO(raw)).convert('RGB')
img.save('OUTPUT.jpg', 'JPEG', quality=88, optimize=True, progressive=True)
print('saved')
"
```

### 4.3 Prompt 写作公式

**结构**：`[Size/aspect] + [Subject] + [Setting/lighting] + [Style keywords] + [Constraints]`

**实测案例（kb cover）**：

```text
A professional blog cover image, 1280x720, 16:9 aspect ratio.
Modern flat-lay photograph of an ergonomic dark walnut wooden keyboard riser
on a clean light desk. Next to the riser, a single manicured hand with long
glossy acrylic nails rests gently on a mechanical keyboard, demonstrating
comfortable typing posture. Soft natural window light from the left, shallow
depth of field, minimalist Scandinavian office background, muted earth tones
with a soft teal accent. Editorial style, no text overlay, no logo, no people
in frame except the hand. Subtle bokeh in the background. High-end product
photography look. Suitable for a niche e-commerce case study blog header.
```

**实测案例（menswear cover, photorealistic + bright daylight）**：

```text
A photorealistic photograph, 1280x720, 16:9 aspect ratio.
Bright natural daylight streaming through a large glass storefront of a
small Southeast Asian menswear retail shop. Neatly hung button-down shirts
and folded trousers on clean white shelving, polished concrete floor
reflecting soft daylight, a simple wooden counter in the center, mid-morning
sun creating warm highlights and gentle shadows, the storefront opens to a
bustling tropical street visible through the glass, real-world documentary
photography, sharp focus, vibrant yet natural colors, no text overlay, no
logo, no people in frame, suitable for a cross-border e-commerce case study
blog header.
```

**必含约束**：
- `no text overlay, no logo, no people in frame except [subject]`——避免生成水印或意外人脸
- `Suitable for a [content type] blog header`——引导风格
- **全英文 prompt**——模型对英文 prompt 理解更准
- **写实场景用 `photorealistic photograph` 关键词**（而非 "professional blog cover image" 这种偏插画风的）——对于零售店/办公室/工厂等真实场景，photorealistic 直出更可信
- **明亮自然光用 `bright natural daylight` + `mid-morning sun`**——避免模型默认走暖色钨丝灯或夜景

### 4.4 Alt 文本写作公式

**结构**：`[主对象] + [动作/状态] + [场景] + [关键词变体]`

**实测案例**：

```yaml
# ✅ 60 字符，SEO 友好
alt: "Custom tall keyboard riser for typing with long acrylic nails"

# ✅ 65 字符
alt: "1,200 sqm kitchen supply wholesale warehouse in Vientiane, Laos"
```

**避坑**：
- 不要 `alt: "image"` / `alt: "cover"` / `alt: "photo"`（零信息）
- 不要堆砌 5+ 个关键词（spam）
- 不要写 "Image of..." / "Photo of..."（爬虫已经知道是图）

### 4.5 验证 alt 真的进了 `<img>` 标签

```bash
grep -oE '<img[^>]*alt="[^"]*"' public/posts/POST-SLUG/index.html
```

如果返回 `MISSING`，检查 front matter 的 `cover.alt` 字段是否拼写正确。

### 4.6 封面图的迭代生成（不满意的常见场景）

第一版封面图**常常不是终版**。如果首图不满足"写实 + 明亮"，**不要降级接受**——直接修 prompt 重跑。

**常见首图问题与第二轮 prompt 调整**：

| 首图症状 | 原因 | 第二轮 prompt 加什么 |
|---|---|---|
| 偏暗 / 室内钨丝灯 | 模型默认走"咖啡馆 / 商店"暖色 | `bright natural daylight` + `mid-morning sun` |
| 像插画 / 3D 渲染 | 默认走 editorial 风格 | `photorealistic photograph` + `real-world documentary photography` |
| 颜色饱和度过高 / 像广告 | 默认走"product shot"风格 | `vibrant yet natural colors` + `sharp focus` |
| 莫名出现人脸 / 文字 | 没强约束 | `no text overlay, no logo, no people in frame` |
| 模糊 / 像素感低 | size 参数过小 | 显式指定 `1280x720` + `sharp focus` |

**两次生成的实测对比**（menswear cover）：

```text
# ❌ 第一版（钨丝灯 + muted tones，提交后判定不合格）
A professional blog cover image, 1280x720, 16:9 aspect ratio. Interior shot
of a small Southeast Asian menswear retail shop, neatly organized shirts and
trousers on open clothing racks, soft warm tungsten lighting, ...

# ✅ 第二版（photorealistic + bright daylight，重跑即合格）
A photorealistic photograph, 1280x720, 16:9 aspect ratio. Bright natural
daylight streaming through a large glass storefront of a small Southeast
Asian menswear retail shop. Neatly hung button-down shirts and folded
trousers on clean white shelving, polished concrete floor reflecting soft
daylight, a simple wooden counter in the center, mid-morning sun creating
warm highlights and gentle shadows, ...
```

**commit 规范**：迭代生成的封面图走独立 commit：

```text
feat(cover): regenerate <slug> cover with photorealistic bright daylight

Previous cover was shot under warm tungsten lighting with muted tones.
Replaced with a photorealistic mid-morning natural daylight scene: ...

File: <OLD_SIZE> KB → <NEW_SIZE> KB (still well under 200 KB LCP target)
Dimensions: 1280x720 JPG unchanged
```

---

## 5. 收尾：语言清理与单位补全

### 5.1 残留 CJK 字符清理

**搜索**：

```bash
python3 -c "
with open('content/posts/POST-SLUG.md') as f:
    body = f.read()
cn = sorted({c for c in body if '\u4e00' <= c <= '\u9fff'})
print('CJK chars found:', cn)
"
```

**如发现**（如 `餐具` / `厨房伴侣`），按以下规则替换：

| 场景 | 替换 |
|---|---|
| 普通名词 | `餐具` → `dishware` / `桌子` → `table` |
| 注释性中文（如 `kitchen companion (厨房伴侣)`） | **删除中文括号** |
| 行内概念解释（如 `shelf-based e-commerce (货架电商)`） | **删除括号 + 括号内的中文**——英文词已自解释，括号是冗余 |
| 同上变体（如 `livestream commerce (直播电商)`） | 同上处理 |
| 翻译术语 | 查英文标准译法（参考 Wikipedia / 行业 SaaS 文档） |

**实测案例（from-shenzhen-university 优化）**：

```markdown
# ❌ 优化前（行内括号解释 CJK 概念，2 处残留）
He grew up inside the Chinese e-commerce ecosystem — from the early days
of Taobao (货架电商, shelf-based e-commerce) through the explosion of
livestream commerce (直播电商).

# ✅ 优化后（删除括号，英文自然流）
He grew up inside the Chinese e-commerce ecosystem — from the early days
of Taobao's shelf-based commerce through the explosion of livestream commerce.
```

**项目先例**：见 `git log --oneline \| grep "remove Chinese terms"`——`2ea75ff fix: remove Chinese terms from Amazon refined-selection post`。

### 5.2 货币与单位显式化

数字前必须明确单位，避免读者混淆（人民币 RMB vs 当地货币 LAK vs 美元 USD）：

**货币单位**：

```markdown
# ❌ 模糊
| Lao buyers | Ask 800,000 → offer 750,000 |
| Chinese buyers | Ask 1,000,000 → offer 400,000 |

# ✅ 显式
| Lao buyers | Ask 800,000 RMB → offer 750,000 RMB |
| Chinese buyers | Ask 1,000,000 RMB → offer 400,000 RMB |
```

**面积单位**（老挝仓库/店面等场景）：

```markdown
# ❌ 模糊（"1,200 square meters" 重复 + 占用字数）
Annual rent (1,200 square meters) | ~300,000 RMB

# ✅ 显式（用 sqm 缩写 + 首次出现注明）
Annual rent (1,200 sqm) | ~300,000 RMB
```

**USD 等价换算**（海外受众必加）：

```markdown
# ❌ 海外读者算不出
Total investment (with inventory) | ~2,000,000 | Ongoing operational capital

# ✅ 关键金额加 USD 换算
Total investment (with inventory) | ~2,000,000 RMB (~$280K USD) | Ongoing operational capital
```

### 5.3 日期 / 修订记录

如果文章被**二次修订**（即优化指南中的 SEO 整改），**在 front matter 加** `lastmod` 字段（PaperMod 会自动渲染在文章页与 sitemap）：

```yaml
date: 2026-05-29        # 原始发布日
lastmod: 2026-06-07     # 上一次优化日
```

**适用判断**：
- ✅ 优化 SEO 字段、改 H2、补 E-E-A-T → **加** `lastmod`
- ✅ 修 CJK 错别字、补外链 → **加** `lastmod`
- ❌ 仅 git 内部 typo 修正 → 不加（避免给 Google 发"内容刚更新"的假信号）

**实测案例**：在 `from-shenzhen-university-to-laos-clothing-empire.md` 优化时：

```yaml
# 优化前
date: 2026-05-29

# 优化后
date: 2026-05-29
lastmod: 2026-06-07
```

---

## 6. 构建与验证

### 6.1 构建命令

```bash
cd /home/wumu/projects/mailmineragent-blog
rm -f .hugo_build.lock
hugo --gc --minify
```

期望输出：`Pages 96 | Static files X | Total in 200ms 左右 | 0 errors`。

### 6.2 SEO 健康度验证脚本

构建后跑这个 Python 脚本（复制即用）：

```python
import re, os
SLUG = "POST-SLUG"
path = f"public/posts/{SLUG}/index.html"
with open(path) as f:
    html = f.read()

# Title
t = re.search(r'<title>([^<]+)</title>', html).group(1).replace(' | MailMiner Agent Blog', '')
print(f'TITLE  ({len(t):3d} ch): {t}')
print(f'  status: {"OK" if 50 <= len(t) <= 60 else "REVIEW"}')

# Description
d = re.search(r'name=description content="([^"]+)"', html).group(1)
print(f'\nDESC   ({len(d):3d} ch): {d}')
print(f'  status: {"OK" if 120 <= len(d) <= 160 else "REVIEW"}')

# Keywords count
k = re.search(r'name=keywords content="([^"]+)"', html).group(1)
print(f'\nKW     ({len(k.split(",")):3d} items)')

# Heading structure
h1, h2, h3 = [len(re.findall(f'<h{i}[^>]*>', html)) for i in (1, 2, 3)]
print(f'\nH1: {h1}  H2: {h2}  H3: {h3}')
print(f'  H1 status: {"OK" if h1 == 1 else "FIX (target 1)"}')

# Image alt in <img>
imgs = re.findall(r'<img[^>]*alt="[^"]*"', html)
print(f'\nIMG alt: {len(imgs)} found')
for img in imgs:
    print(' ', img[:200])

# OG / Twitter / JSON-LD image
for label, pat in [('og:image', r'property="og:image" content="([^"]+)"'),
                   ('twitter:image', r'name=twitter:image content="([^"]+)"'),
                   ('jsonld image', r'"image":"([^"]+)"')]:
    m = re.search(pat, html)
    print(f'  {label}: {m.group(1) if m else "MISSING"}')

# Read time
rt = re.search(r'(\d+) min', html).group(0)
print(f'\nRead time: {rt}')

# CJK residue
cn = sorted({c for c in html if '\u4e00' <= c <= '\u9fff'})
print(f'\nCJK chars in body: {len(cn)} -> {set(cn)}')
```

### 6.3 必须通过的硬性指标

| 指标 | 通过条件 |
|---|---|
| Build 0 errors | `hugo` 无错误输出 |
| H1 count | 恰好 1 |
| Title length | 50-60 字符 |
| Description length | 120-160 字符 |
| `<img alt="...">` | 至少 1 个（cover 图） |
| CJK chars in body | 0 |
| `og:image` / `twitter:image` / JSON-LD `image` | 全部 3 个都解析到正确 URL |

### 6.4 验证外链 / 内链可达

```bash
python3 -c "
import re, html as htmllib
with open('public/posts/POST-SLUG/index.html') as f:
    raw = f.read()
m = re.search(r'<div class=\"post-content[^>]*\">(.*?)<footer', raw, re.DOTALL)
body = htmllib.unescape(m.group(1))

hrefs = re.findall(r'href=(?:\"([^\"]+)\"|([^\s>]+))', body)
print('=== External (non-mailminer, non-share) ===')
EXCLUDE = ['api.whatsapp','facebook.com','reddit.com','telegram.me',
           'linkedin.com','x.com','news.ycombinator','gohugo.io',
           'mailminer.work','mailmineragent.com']
for a, b in hrefs:
    u = a or b
    if u.startswith('http') and not any(e in u for e in EXCLUDE):
        print(' -', u)
print()
print('=== Internal ===')
for a, b in hrefs:
    u = a or b
    if u.startswith('/') and not u.startswith('//'):
        print(' -', u)
"
```

> **常见 minify 陷阱**：Hugo `--minify` 输出的 HTML 把 `href="..."` 变成无引号 `href=...`，**用 `href=(?:"([^"]+)"|([^\s>]+))` 正则**才能匹配两种格式。

---

## 7. 提交规范

### 7.1 Commit message 模板

```
<type>(<scope>): <subject>

<body>
```

| type | 用途 |
|---|---|
| `feat(seo)` | 文本内容 / front matter / 内链外链优化 |
| `feat(cover)` | 封面图替换 / 新增 |
| `feat(about)` | about 页结构改动 |
| `chore(assets)` | 静态资源清理（删除未引用的图片） |
| `fix(content)` | 修正事实错误 / 错别字 / CJK 残留 |

**实测 commits**（参考 `git log --oneline -10`）：

```
feat(cover): replace PNG placeholder covers with real AI-generated JPGs
feat(seo): overhaul kitchen-supply Laos post for SEO + E-E-A-T
feat(about): add support email contact to about page
chore(assets): remove unused cover_deepseek_cache.jpg
```

### 7.2 Commit body 必含项

1. **改动前的状态**（字符数 / 字段缺失等具体数据）
2. **改动后的状态**（具体数字 + 验证命令）
3. **影响的文件**（列路径）

### 7.3 Push 前的 sanity check

```bash
# 1. 确认 staged 文件正确
git status --short
# 2. 确认 diff 合理
git diff --cached --stat
# 3. 确认远端同步
git rev-parse HEAD origin/main
# 4. push
git push origin main
```

---

## 8. 常见错误清单

| 错误 | 症状 | 修复 |
|---|---|---|
| **双 H1** | `H1 count: 2` | 删除 markdown 顶部手动 H1，保留 TL;DR |
| **Description 超长** | 描述字符数 170+ | 压缩到 120-160，删去 "and" / "the" 等连接词 |
| **Title 超长** | 字符数 60+ | 删除副标题，把数字 / 关键词前置 |
| **CJK 残留** | body 含中文字符 | 替换为英文 / 删除括号注释 |
| **og:image 404** | 图片引用 .png 但实际是 .jpg | front matter `cover.image` 后缀与实际文件一致 |
| **Description 渲染成空** | regex 漏匹配 | Hugo minify 后用 `name=description content="..."`（无空格等号格式） |
| **外链 minify 后搜不到** | 用 `href="..."` 匹配失败 | 用 `href=(?:"([^"]+)"\|([^\s>]+))` 双格式正则 |
| **封面图太大** | 单图 1MB+ | 用 Pillow 转 JPG quality=88，目标 < 200KB |
| **E-E-A-T 段落无内链** | Past coverage 段是纯文字 | 加 2-3 个 `/posts/...` 链接 |
| **未指定 `--ref`** | 误以为需要参考图 | 封面图**纯文生图**即可，`ref=None` |
| **封面图偏暗 / 像插画** | 首图用 "professional blog cover image" + 默认钨丝灯 | 改用 `photorealistic photograph` + `bright natural daylight` 重跑 |
| **首版封面图接受** | 写实/明亮度不达标也直接 commit | 不达标就 commit 浪费一次，**重跑 prompt 走独立 commit** |
| **front matter `keywords` 写成 YAML 数组** | Hugo 解析失败 | 写成单行 `keywords: ["a", "b", "c"]` 而非多行 block |
| **未更新 `cover.image` 引用** | 替换了图片但 front matter 还是旧路径 | sed 替换 `image: "images/OLD-NAME.png"` → `image: "images/NEW-NAME.jpg"` |

---

## 附录 A：完整改动 checklist

优化一篇文章时，按这个顺序执行（每步完成打勾）：

- [ ] 读完整篇文章，识别核心数字、人物、关键术语
- [ ] **Module A**：更新 front matter（title, description, keywords, author, cover）
- [ ] **Module B**：删除手动 H1，添加 TL;DR，重写 H2 含长尾词
- [ ] **Module C**：补充 3-5 个外链，添加 2-3 个内链，添加文末 E-E-A-T section
- [ ] **Module D**：生成 1280×720 JPG 封面图（80-200KB），更新 front matter `cover.image`
- [ ] **Cleanup**：移除 CJK 字符，补全货币单位
- [ ] **Build**：`hugo --gc --minify` 0 errors
- [ ] **验证**：跑 §6.2 脚本，所有硬性指标通过
- [ ] **Commit**：`feat(seo): ...` 格式，body 含 before/after 数字
- [ ] **Push**：`git push origin main`

---

## 附录 B：相关文件位置

```
mailmineragent-blog/
├── config.toml                            # site-level 配置（一般不动）
├── content/
│   ├── about.md                           # 团队介绍页（链向这里！）
│   ├── search.md                          # 搜索页
│   └── posts/
│       ├── keyboard-riser-niche-tiktok-hustle.md        # 已优化参考
│       ├── kitchen-supply-wholesale-laos-sichuan-entrepreneur.md  # 已优化参考
│       └── from-shenzhen-university-to-laos-clothing-empire.md    # 已优化参考（与 kitchen-supply 互为 series）
├── static/
│   └── images/                            # 所有封面图、OG image 实际文件
│       ├── banner2.png                    # 未跟踪，与任务无关
│       ├── cover_deepseek_cache.jpg       # 历史资产，保留或删除按需
│       ├── keyboard-riser-tiktok-hustle-cover.jpg
│       ├── kitchen-supply-wholesale-laos-sichuan-cover.jpg
│       └── shenzhen-grad-vientiane-menswear-cover.jpg
├── themes/PaperMod/                       # ⚠️ 不要修改（升级会冲掉）
├── layouts/partials/extend_footer.html    # 项目级 override（51.la 统计）
└── docs/
    └── POST-OPTIMIZATION-GUIDE.md         # 本文档
```

---

## 附录 C：参考资源

- 9router image API: `~/.agents/utils/router_i2i.py`（本地 CLI）
- Google Search Central: https://developers.google.com/search
- PaperMod 文档: https://github.com/adityatelange/hugo-PaperMod
- E-E-A-T 官方说明: https://developers.google.com/search/docs/fundamentals/creating-helpful-content
