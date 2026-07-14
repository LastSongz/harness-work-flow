# Harness / SDD 工作流探索

本仓库用于为华为海思供应链项目组探索一套可落地的 Harness / Spec-Driven Development（SDD）工作流。

当前版本为 `v0.1`，聚焦以下前提：

- 一个业务需求对应一个代码仓库；
- 需求分析和反串讲完成后，先形成可评审的最小 Spec；
- Spec 与代码同库，通过 Git 记录版本，通过 PR 完成评审；
- 开发中的新发现持续回写 Spec；影响业务行为的变更必须先批准 Spec，再修改代码；
- AI 根据“已批准 Spec + Spec 差异 + 当前代码”做增量开发，不全量重写代码。

## v0.1 包含什么

- [流程说明](docs/workflow/sdd-workflow-v0.1.md)：从原始需求到交付的完整路径；
- [设计说明](docs/superpowers/specs/2026-07-14-harness-sdd-v0.1-design.md)：目标、原则、边界和后续演进；
- [Spec 模板](templates/spec/)：需求、设计、任务和决策记录；
- [评审清单](templates/review/)：Spec PR 与 Code PR 的门禁检查项。

## 推荐的业务仓库结构

```text
<业务代码仓库>/
├── specs/
│   └── <需求编号>-<简短名称>/
│       ├── requirements.md
│       ├── design.md
│       ├── tasks.md
│       └── decisions.md
├── src/
└── tests/
```

`requirements.md` 是业务行为的权威描述；`design.md` 和 `tasks.md` 是可随实现演进的派生资产；`decisions.md` 用于收敛开发期间产生的澄清，避免事实散落在聊天和看板中。

## 最小试点方式

1. 选择一个边界清晰、预计 3～10 个开发日的小需求。
2. 将 `templates/spec/` 复制到业务仓库的 `specs/<需求编号>-<简短名称>/`。
3. 完成最小 Spec，并用 Spec PR 组织业务、开发、测试评审。
4. Spec 合入后再开始实现；Code PR 必须引用对应 Spec 路径和基线提交。
5. 交付后复盘 Spec 变更次数、返工次数、遗漏问题和交付周期。

## 当前不包含

- CodeArts、GitLab 或 Gerrit 的专属插件；
- 自动生成代码的固定 AI 工具；
- 多仓库需求编排；
- 紧急线上问题的特殊审批流程；
- 用 Spec 全量生成或覆盖存量代码。
