# VibeCoding 高效 AI 辅助开发课程总纲

> [!summary]
> 20 课、每课 25～30 分钟，以 Spring Boot 订单系统的“订单取消”功能贯穿，目标是建立工具无关、证据驱动、可以持续改进的 AI 辅助开发工作流。

## 课程目标

完成课程后，你能够：

1. 区分 VibeCoding、AI 结对编程和 Agentic Engineering，知道哪些决策必须由人负责。
2. 把模糊需求整理为包含范围、约束、验收、验证和停止条件的任务简报。
3. 先探索代码库，再产出最小可执行 Spec、文件级 Plan 和可独立验证的小任务。
4. 用测试、编译、静态检查、运行日志和 Diff 驱动“检查→修改→验证→修正→停止”循环。
5. 安全使用 Git、Worktree、多 Agent、Rules、Skills、Hooks 和 MCP，并为权限与失败设置边界。
6. 独立完成一次 Spec→Plan→Tests→Code→Review→Commit/PR 的完整演练。

## 适用人群

- 已能独立完成常规开发，希望系统掌握 AI 辅助开发的工程师。
- 正在使用 Codex、Claude Code、Cursor 或 GitHub Copilot，但结果稳定性不高的开发者。
- 需要把个人经验沉淀为团队规则、评审清单或 Harness 的技术负责人。
- 示例使用 Java/Spring Boot，但核心方法同样适用于前端、Python、Go 和其他技术栈。

## 前置条件

- 理解 Git 分支、提交、Diff 和 Pull Request 的基本概念。
- 能读懂 Spring Boot 的 Controller、Service、Repository 和测试代码。
- 本地有一个可运行的练习仓库，或用纸面方式完成前 10 课的资产练习。
- 至少准备一种能搜索文件、编辑代码、运行命令的 AI 编码 Agent。

## 贯穿案例：Spring Boot 订单取消

课程假设存在一个分层订单系统：

```text
POST /api/orders/{orderId}/cancel
        ↓
OrderController
        ↓
OrderApplicationService
        ↓
Order 聚合 / OrderRepository
        ↓
RefundGateway（已支付订单） + 审计日志 + OrderCanceledEvent
```

统一业务基线：

- `CREATED`、`PAID` 状态可以取消，`SHIPPED`、`CANCELED` 不可重复改变状态。
- `PAID` 订单取消时创建退款请求；外部退款失败不能留下不一致的本地状态。
- 请求携带 `requestId`，重复请求返回同一业务结果，不重复退款。
- 使用版本号或等价机制处理并发取消。
- 记录操作者、原因、时间、原状态和结果；日志不得泄露敏感数据。
- 先通过单元测试表达核心规则，再补契约/集成验证。

这是一条教学基线，不代表唯一正确设计。每一课都会说明本次练习是在澄清、规划、实现、验证还是治理该功能。

## 学习方式

每课采用同一节奏：

1. **定位（3～5 分钟）**：说明本课在主工作流中的位置。
2. **讲解（6～8 分钟）**：建立一个可迁移的方法。
3. **示例与步骤（8～10 分钟）**：围绕订单取消展示具体产物。
4. **练习（5～7 分钟）**：亲手写一小段 Spec、Plan、测试或检查记录。
5. **验收与小结（2～3 分钟）**：用明确标准判断是否完成。

不要复制“万能提示词”。先理解任务，再按当前仓库补齐必要字段；任何 AI 输出都要用工具反馈或人工审查验证。

## 20 课一览

| 课次 | 主题 | 本课产物 | 建议时长 |
|---|---|---|---:|
| 01 | [[01-从VibeCoding到Agentic Engineering]] | 人机职责与风险边界表 | 28 分钟 |
| 02 | [[02-Agent Harness原理]] | Harness 六要素检查表 | 28 分钟 |
| 03 | [[03-工具和模型选择]] | 任务路由矩阵 | 28 分钟 |
| 04 | [[04-高质量任务简报]] | 订单取消任务简报 | 30 分钟 |
| 05 | [[05-AI阅读陌生代码库]] | 代码地图、调用链和影响面 | 30 分钟 |
| 06 | [[06-最小可执行Spec]] | 最小可执行 Spec | 30 分钟 |
| 07 | [[07-Plan Before Code]] | 方案比较与文件级 Plan | 30 分钟 |
| 08 | [[08-任务拆分]] | 垂直切片任务板 | 28 分钟 |
| 09 | [[09-上下文工程]] | 相关上下文包与交接摘要 | 28 分钟 |
| 10 | [[10-项目长期记忆]] | 最小项目规则草案 | 28 分钟 |
| 11 | [[11-AI与TDD]] | Red-Green-Refactor 测试切片 | 30 分钟 |
| 12 | [[12-核心执行循环]] | 一轮可验证执行日志 | 28 分钟 |
| 13 | [[13-证据驱动调试]] | 复现记录、假设表和回归测试 | 30 分钟 |
| 14 | [[14-AI代码评审]] | 分层 Diff 评审意见 | 30 分钟 |
| 15 | [[15-Git安全工作流]] | 窄变更与恢复方案 | 28 分钟 |
| 16 | [[16-多Agent并行]] | 所有权与合并计划 | 30 分钟 |
| 17 | [[17-Best-of-N与独立裁判]] | 多方案评分表 | 28 分钟 |
| 18 | [[18-扩展机制职责边界]] | Rules/Skills/Hooks/MCP 路由表 | 30 分钟 |
| 19 | [[19-安全自治与持续改进]] | 权限矩阵与质量指标 | 30 分钟 |
| 20 | [[20-结业实战]] | 完整交付证据包 | 30 分钟 |

## 主工作流

```text
探索与澄清
  → 最小可执行 Spec
  → 方案比较与文件级 Plan
  → 垂直切片和小任务
  → Tests / Code 的可验证循环
  → 独立 Diff 审查
  → 窄 Git 变更与 PR
  → 把重复经验沉淀为 Rules / Skills / Hooks
  → 在权限、沙箱和质量门禁内提高自治与并行
```

关键原则：

- **先读后写**：不知道入口、调用链和约束时，不授权大范围修改。
- **意图可追踪**：Spec 说“要实现什么”，Plan 说“准备怎么改”，测试和 Diff 提供证据。
- **小步可恢复**：每个任务都应有检查点、停止条件和回滚路径。
- **反馈优先**：编译、测试、静态分析、运行日志和真实界面比模型自我评价更可靠。
- **独立审查**：实现者不应是唯一裁判；高风险变更必须有人类最终确认。
- **渐进增强**：先把手工循环做对，再把重复动作固化为规则、技能、Hook 或自动化。

## Harness Engineering 符合性映射

| Harness 工程要求 | 对应课程 | 可检查证据 |
|---|---|---|
| 模型、指令、工具、上下文、权限和反馈协同 | 02、03、09 | Harness 清单、任务路由矩阵、上下文包 |
| 先探索和澄清，不凭文件名或提示词猜实现 | 04、05 | 任务简报、代码地图、已确认事实与未知问题 |
| 意图持久化为 Spec、Plan 和可执行任务 | 06、07、08 | 最小 Spec、文件级 Plan、垂直切片任务板 |
| 以测试和真实工具输出驱动执行 | 11、12、13 | 失败测试、执行日志、复现与回归证据 |
| 独立审查和可恢复交付 | 14、15、17 | 分层评审、窄 Diff、恢复点、多方案评分 |
| 并行工作具备所有权、隔离和汇总机制 | 16 | Agent 所有权表、Worktree 与合并顺序 |
| 长期记忆和扩展机制按职责渐进沉淀 | 10、18 | 项目规则、Rules/Skills/Hooks/MCP 路由表 |
| 自治受沙箱、最小权限、CI 和质量指标约束 | 19、20 | 权限矩阵、质量门禁、完整交付证据包 |

如果某项只听懂概念却拿不出对应证据，就视为尚未达到 Harness 工程要求，需要回到对应课程重做练习。

## 结业标准

结业实战必须同时满足：

- 有明确的任务简报、最小可执行 Spec、文件级 Plan 和任务拆分。
- 关键业务规则至少有单元测试，接口/持久化等跨层风险有相应验证。
- 实施记录能说明每轮命令、结果、修正和停止依据。
- Diff 范围与 Spec 一致，没有混入无关格式化或用户现有改动。
- 完成架构、异常、安全、性能、数据库/兼容性检查。
- Git 操作可恢复，Commit/PR 描述包含验证证据和剩余风险。
- 能解释为何使用或不使用多 Agent、Best-of-N、Rules、Skills、Hooks 与 MCP。

## 推荐学习节奏

- **稳健节奏**：每周 4 课，5 周完成；每学完 5 课整理一次产物。
- **集中节奏**：每天 2 课，10 天完成；两课之间保留 10 分钟休息。
- **实战节奏**：前 10 课用纸面案例，后 10 课在真实练习仓库复做。

如果某课练习未通过验收，不必追求“当天刷完”。先修正产物，再进入下一课，能够复现和验证比完成速度重要。

## 官方实践依据

课程取以下官方资料的交集，而不是绑定某一产品界面：

- [OpenAI：How OpenAI uses Codex](https://openai.com/business/guides-and-resources/how-openai-uses-codex/)：先 Ask/Plan、Issue 式任务描述、AGENTS.md、Best-of-N 与迭代环境。
- [Cursor：Best practices for coding with agents](https://cursor.com/blog/agent-best-practices)：Agent Harness、Plan Mode、上下文管理、Rules、Skills、Hooks 和 TDD。
- [Claude Code：Common workflows](https://code.claude.com/docs/en/common-workflows)：探索陌生代码、测试、验证、Worktree 并行和 Plan Before Editing。
- [Claude Code：Features overview](https://code.claude.com/docs/en/features-overview)：CLAUDE.md、Skills、Hooks、MCP、子 Agent 与团队的职责边界。
- [GitHub Copilot：Get the best results](https://docs.github.com/en/copilot/tutorials/cloud-agent/get-the-best-results)：小而清晰的任务、验收条件、研究/计划/迭代、仓库指令和 Agent 环境。
- [GitHub Spec Kit](https://github.github.com/spec-kit/)：Spec→Plan→Tasks→Implement 的结构化资产链。
- [Spec Kit：Spec persistence models](https://github.github.com/spec-kit/concepts/spec-persistence.html)：Spec-first、Spec-anchored、Spec-as-source 以及变更后的资产维护策略。

---

导航：[[VibeCoding课程首页]] · 下一课 [[01-从VibeCoding到Agentic Engineering]] · 相关 [[项目组AI-SDD综合报告-v1.0]]
