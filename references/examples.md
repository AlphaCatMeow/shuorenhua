# 改写示例

> 每个示例展示同一段内容的 AI 版和人话版。

## 中文示例

### 示例 1：项目介绍

**AI 版：**
> 该项目是一个创新性的解决方案，旨在通过深度整合多种前沿技术，为用户提供全方位、一站式的智能化体验。它不仅能够显著提升工作效率，还能有效降低运营成本，实现真正的降本增效。

**人话版：**
> 这个项目把语音识别和自动翻译接到一起，用户说中文就能直接出英文字幕。上线两周日活 1200，翻译准确率 94%。

**改了什么：**
- 删掉"创新性""前沿技术""全方位""一站式""智能化"——全是空词
- 删掉"不仅…还能…"的二元结构
- 加了具体功能描述和数据

---

### 示例 2：技术总结

**AI 版：**
> 综上所述，通过对系统架构的全面优化和持续迭代，我们在性能、安全性和可维护性等方面均取得了显著提升。这一成果充分体现了团队在技术创新方面的不懈追求和卓越实力。

**人话版：**
> 这轮改完之后：API 响应时间从 800ms 降到 120ms，修了 3 个 SQL 注入漏洞，把 6000 行的 God Class 拆成了 12 个模块。

**改了什么：**
- 删掉"综上所述"和整个总结式开头
- "显著提升"换成具体数据
- 删掉"充分体现""不懈追求""卓越实力"——自吹自擂
- 用具体改动代替抽象描述

---

### 示例 3：消息回复

**AI 版：**
> 好问题！这确实是一个值得深入探讨的话题。让我来为你详细解释一下。首先，我们需要了解的是，这个问题的本质在于……

**人话版：**
> 核心原因是缓存没失效。用户改了头像但 CDN 还在用旧的，TTL 设了 7 天太长了。改成 1 小时就行。

**改了什么：**
- 删掉全部谄媚和铺垫
- 直接给原因和解决方案

---

### 示例 4：新闻摘要

**AI 版：**
> 在当今快速发展的人工智能领域，OpenAI 近日发布了其最新的大语言模型，引发了业界的广泛关注和热烈讨论。该模型在多个关键指标上实现了显著突破，标志着人工智能技术迈入了一个全新的发展阶段。

**人话版：**
> OpenAI 发了 GPT-5。MMLU 跑分 92.3%，比上一代高 7 个点。上下文窗口翻倍到 256K。API 价格没涨。

**改了什么：**
- 删掉"在当今""快速发展""广泛关注""热烈讨论"——全是废话
- "显著突破"换成具体跑分
- "全新的发展阶段"删掉——读者自己会判断

---

## English Examples

### Example 1: Product description

**AI version:**
> Our groundbreaking platform serves as a testament to the transformative potential of AI, empowering teams to navigate complex challenges and unlock unprecedented levels of productivity. Nestled at the intersection of innovation and practicality, it showcases how cutting-edge technology can foster meaningful collaboration.

**Human version:**
> The platform auto-assigns tickets based on who fixed similar bugs before. Teams using it close issues 2 days faster on average.

**What changed:**
- Removed "groundbreaking", "testament", "empowering", "navigate", "unprecedented", "nestled", "showcases", "cutting-edge", "foster"
- Replaced vague claims with specific functionality and data

---

### Example 2: Technical update

**AI version:**
> We're excited to announce a comprehensive update that significantly enhances performance, bolsters security, and streamlines the developer experience. This pivotal release underscores our commitment to delivering robust, scalable solutions.

**Human version:**
> This release cuts cold start time by 60%, patches CVE-2024-3891, and drops the config from 200 lines to 40. Upgrade guide is in the changelog.

**What changed:**
- "Comprehensive update" → specific changes
- "Significantly enhances" → "cuts by 60%"
- "Bolsters security" → specific CVE
- "Streamlines developer experience" → specific config reduction
- Deleted "pivotal", "underscores", "commitment", "robust", "scalable"

---

### Example 3: Analysis (two-pass demo)

**AI version:**
> The landscape of remote work has undergone a profound transformation. It's not just about working from home — it's about reimagining the very fabric of how we collaborate. Companies that fail to navigate this paradigm shift risk being left behind in an increasingly competitive ecosystem.

**First pass:**
> More companies went remote after 2020. Some did it well, some didn't. The ones that kept async communication and cut meetings kept their engineers longer.

**Audit — what still feels AI?**
- "Some did it well, some didn't" is vague
- The structure is still a bit too clean

**Final:**
> After 2020, companies that switched to async-first communication and cut recurring meetings below 10 hours/week saw 23% lower attrition among engineers (State of Remote Work Survey, 2023). The ones that just moved their 9-to-5 schedule to Zoom lost people faster than before.

**What changed in second pass:**
- Added specific data source
- Added specific number (10 hours/week, 23%)
- Made the contrast concrete instead of vague
