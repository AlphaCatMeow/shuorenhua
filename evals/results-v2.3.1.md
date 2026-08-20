# v2.3.1 候选验收归档

> 日期：2026-08-19 / 2026-08-20
> 状态：**NOT release-ready**。仓库静态门禁与 HUMAN 标定已完成，两个改写席位均已 111/111；双向 judge 仍缺 DeepSeek 对 Opus B-97–111 一批，而已有证据已发现 2 个 L1 保真失败。

## 1. 本版范围

- 增加 mini / lite / full 三档入口；mini 是 1,500 字符以内的自包含版本。
- 增加 API reference、FAQ 等 Scene Packs，并补相应正反用例。
- benchmark 从 103 条扩到 111 条：61 SF + 50 SNF；blind、map 与 7 批范围同步。
- 增加 HUMAN 长文 residual 对照和严格 manifest 校验；这组数据不进入 rewrite、judge 或 benchmark 分母。
- 加固 plugin / marketplace 元数据结构与版本一致性检查。

## 2. 冻结输入与正式席位

| 输入 | SHA256 |
|------|--------|
| `SKILL.md` | `1b4632060b91a647867c46270b9a71df0511bf0128cde28bce70be084d9d8b62` |
| `references/scene-packs.md` | `dcda3b11b03193b0de2537f452bed9c3e330b99c533b995db59da302f4fbfe74` |
| `evals/benchmark-blind.md` | `79dedd4247e0df8a292a883a282e1d80214e0f6cc829a5855348b6a7e063acdd` |
| `automation/eval/rewrite-prompt.md` | `2d076186242b30a6a53c56559ea0a8a296985fdb7a6641643de5a49aa17ee0c2` |

正式被测席位按维护者后续决定为：

- Claude Code CLI 2.1.237，first-party `claude-opus-5`，`--model opus --effort high`。
- DeepSeek V4 Pro；Cindy Host 实际路由与计费元数据只含 `deepseek-v4-pro[1m]`。

Codex 5.6 Sol 因额度耗尽记“未参与”，没有用其他 Codex 模型替代。

## 3. targeted 结果

Opus mini 6/6 通过；Scene Pack 7 通过、1 个 L2 警告、0 L1，新增 SNF 误杀 0/4。DeepSeek Scene Pack 同为 7 通过、1 个 L2 警告、0 L1。

共同警告是 B-26 / SF-61：两模型都删除了无源的绝对安全保证，保留升级顺序、硬限制和 `2 小时` 待确认，但正文没有先直接回答“现有材料无法证明一定不会丢数据”。这是 FAQ 信息顺序的 L2 执行弱点，不是事实漂移，不改门槛。

Grok targeted 也出现同型 B-26 警告；其输出带额外过程前言，按 L0 格式偏差只作辅助证据，不计正式全量席位。

## 4. 正式全量基线状态

| 链路 | 改写完整性 | judge 完整性 | 已知结果 |
|------|------------|----------------|----------|
| Opus rewrite → DeepSeek judge | 111/111 | 96/111 | L1 失败 1：B-39 / SF-27；缺 B-97–111 judge |
| DeepSeek rewrite → Opus judge | 111/111 | 111/111 | L1 失败 1：B-95 / SF-07；SF L2 44/58；SNF 误杀 1/50 |

Opus B-97–111 先后两次在请求入口收到 429，失败原件保留在 gitignored 运行目录。维护者随后说明订阅已登录；`auth status` 仍给出下列假阴性：

- 时间：`2026-08-20T17:51:30+08:00`
- 命令：`/Users/zhangqi/.local/bin/claude auth status`
- CLI：`2.1.237 (Claude Code)`
- 退出码：1
- stdout：`loggedIn=false / authMethod=none / apiProvider=firstParty`
- binary SHA256：`338901351d4ff17495738c67fc3e12a32c1b506738ac5e012eb782d3d8b5be43`

不再把该命令当作可用性门禁：同一 CLI 的实际 first-party Opus 探针成功，随后 B-97–111 改写和 7 个全新 judge 会话均 `rc=0`，`modelUsage` 唯一 key 为 `claude-opus-5`，`canonicalModel=claude-opus-5`、`provider=firstParty`、无 web search。未重跑已成功的改写批次。

DeepSeek 对 Opus 的最后一批 judge 未补成。原路径是 Cindy Host / Orca Worker，不是 Claude Pro 订阅自带模型；当前可调的 Claude CLI 显式请求 `deepseek-v4-pro` 返回 404 `unrecognized_model`。因此未用 Opus 或其他模型代替 DeepSeek 身份。

## 5. 两个 L1 独立复核

### B-39 / SF-27（Opus 改写）

结论：**Opus 5 模型执行失败；DeepSeek judge 判定正确；无规格歧义；无基础设施致因。**

- blind 输入明确包含“通过系统性治理稳稳兜住高峰期流量”。
- 映射明确为 SF-27；预期逐字要求删除姿态词，但保留“高峰期流量”这一 fallback 适用条件。
- 冻结规则把 code-context 的真实运行行为、适用条件和边界列为 protected spans。
- Opus 自己把整行判断为“纯姿态层”，结果删除“高峰期流量”。raw JSON 与归一化 Markdown 的正文逐字一致。
- 该批 rc=0、stderr 为空、`is_error=false`、`terminal_reason=completed`，`modelUsage` 只有 `claude-opus-5`。唯一权限拒绝是阻止读取 live repo，随后正常读取冻结副本并完整产出 16 条。
- hard metrics 的 7/7 只覆盖数字、日期、反引号等字面 token；语义条件本来就交给 judge，因此不与 L1 判定冲突。

现行门槛要求 L1=0。不得把这条降成 L2、删除用例或只重跑失败项追分。

### B-95 / SF-07（DeepSeek 改写）

结论：**DeepSeek V4 Pro 模型执行失败；Opus 5 judge 判定正确；无规格歧义。**

- 原文只说试验“展示 cloud-native architecture 的潜力”，这是可能性，不是实施状态。
- 改写变成 `The platform is built on cloud-native architecture`，新增了“平台已基于该架构构建”的关系。
- benchmark 明确禁止新增实施关系；Opus judge 将其判为 hard failure。主线在 judge 前已独立标记同一风险，结论相互独立且一致。

Opus judge 对 DeepSeek 全 111 条的机械汇总为：L1 `1`；SF L2 `44/58 = 75.9%`（三条 L3 不进分母）；SNF 误杀 `1/50 = 2.0%`，低于 `<10%` 门槛。但 L1 门槛已经失败。

## 6. HUMAN 长文对照与许可

本轮 HUMAN 调整为 8 篇分层公开文本：3 篇现代中文公开年度/报道回顾、2 篇现代英译中公开采访、3 篇历史中文原作。正文从 2018–2021 年的固定 MediaWiki revision wikitext 机械抽取，避免旧页面渲染时混入当前模板；manifest 保留原作者/贡献者、固定来源与日期、CC BY / CC BY-SA 4.0、逐篇许可证据、抽取说明、原始语言和 AI 依据。正文及其改编不适用根目录 MIT。

代表性边界：这批语料覆盖 7 个作者组、3 个功能场景、3 篇历史 + 5 篇现代、6 篇中文原作 + 2 篇英译中。`status` 仍只是公开年度/报道回顾的功能近似，不等同于内部团队周报；`public-writing` 两篇都是翻译采访；仍缺真实团队聊天、内部周报和现代原创中文技术文档。它是分层 residual 切片，不代表现代中文职场写作，也不能单独支撑“人味”阈值。

| ID | 作品 | 作者组 | 场景 | 汉字 | 句数 |
|----|------|--------|------|-----:|-----:|
| HUMAN-01 | 維基新聞2020年台灣大事回顧 | Wikinews Taiwan review | status | 3,510 | 67 |
| HUMAN-02 | 維基新聞2020年香港大事回顧 | Wikinews Hong Kong review | status | 1,856 | 51 |
| HUMAN-03 | 維基新聞2020年原創報道回顧 | Wikinews original reporters | status | 6,149 | 121 |
| HUMAN-04 | 維基新聞採訪比爾·漢蒙斯 | William S. Saturn / Kit Wong | public-writing / translated | 2,485 | 61 |
| HUMAN-05 | 維基新聞採訪喬·喬根森 | William S. Saturn / contributors | public-writing / translated | 3,030 | 98 |
| HUMAN-06 | 上李鴻章書 | 孫中山 | docs / historical | 6,819 | 254 |
| HUMAN-07 | 文學改良芻議 | 胡適 | docs / historical | 5,055 | 246 |
| HUMAN-08 | 我之節烈觀 | 魯迅 | docs / historical | 4,809 | 210 |

总体分布：句长 CV `n=8 / min=0.43 / median=0.52 / p90=0.61 / max=0.61`；连词密度 `n=8 / min=0.00 / median=1.20 / p90=5.22 / max=8.94`。按场景和长度桶的原始值由 `--human-stats` 完整输出。

HUMAN、SF、SNF 在同场景没有稳定分离证据；尤其各场景 HUMAN 只有 2–3 篇。因此保留 report-only，不设产品阈值。

### 两项 L1 的失败后修复验证

旧的 B-39 / B-95 正式证据继续记为 L1 失败。本轮先冻结 14 条 task-local held-out，覆盖条件删除、关系补强、情态/完成态、纯姿态、no-op、普通黑话和主体归属；冻结后才把保真账本改为按子句/事实要素判断，并增加输入→输出、输出→输入双向回读。

- first-party `claude-opus-5`：H-01–14 全部符合预期，L1 `0/14`；模型 JSON 为 success/completed/end_turn，provider=`firstParty`。
- `grok-4.6-build`：H-01–14 全部符合预期，L1 `0/14`；Santi verifier 确认选择模型 `grok-4.6`、runtime `grok-4.6-build`、fingerprint `fp_08d0bc26c22b024e`。输出前有过程前言，记 L0 格式偏差，不影响内容判定。
- 历史题 targeted：两模型均保留 B-39 的“高峰期流量”以及 B-95 的 `potential/can` 未实现关系，内容 `4/4` 通过。

这组证据只说明修复方向能泛化且覆盖已知题；它不替换旧全量失败，也不能代替完整新全量基线。因此本文件顶部的 **NOT release-ready** 结论不变。

## 7. 仓库门禁状态

- `python3 automation/check_repo.py`：结构计数为 111 用例 / 20 样本 / HUMAN 8 篇 / 24 锚点 / 98 链接 / 3 词表；预期退出 1，HUMAN direct 场景缺 `docs`、`status`。
- `python3 automation/eval/hard_metrics.py --human-stats evals/human-corpus.jsonl`：OK。
- `python3 automation/eval/hard_metrics.py --human-stats evals/human-corpus.jsonl --report-json`：OK。
- `python3 automation/eval/hard_metrics.py --calibrate`：61 SF / 50 SNF / 8 HUMAN，OK；没有新增阈值。
- HUMAN validator 单测、Python 编译、blind 同步、链接与 diff 门禁见最终工作区验证记录。

## 8. 发布判断

v2.3.1 候选不满足 release-ready：

1. 两个改写席位各有 1 个 L1（Opus B-39、DeepSeek B-95），违反 L1=0 的预先冻结门槛。
2. DeepSeek judge 对 Opus 的 B-97–111 未完成；当前会话无 Cindy Host / Orca Worker 调用通道，直接 Claude CLI 不识别该模型。
3. HUMAN 现有 8 篇满足 residual cohort 合同，但 direct 场景只有 public-writing，发布代表性门禁仍缺 docs/status。

因此不写 2.3.1 发布标签，不 bump plugin / marketplace 版本，不宣称双向 judge 已全部闭环。当前对外 release-ready 版本仍是 2.3.0；后续补充 Claude 完整审核并关闭上述门禁后才会正式 release。
