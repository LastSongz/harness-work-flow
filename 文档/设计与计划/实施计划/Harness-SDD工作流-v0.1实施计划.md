# Harness / SDD 工作流 v0.1 实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 交付一套适用于单需求、单仓库场景的渐进式 SDD 流程和可复制模板。

**Architecture:** 以 Markdown 作为可版本化资产，以 Spec PR 和 Code PR 作为两道门禁。v0.1 只实现流程、模板和检查清单，不绑定代码托管平台，也不实现自动化程序。

**Tech Stack:** Markdown、Git、Pull Request。

## Global Constraints

- 所有设计和流程文档使用中文；
- Spec 与业务代码同库；
- 业务行为变更必须先批准 Spec；
- AI 只做基于 Spec Diff 和当前代码的增量修改；
- v0.1 不实现多仓库编排和平台插件。

---

### Task 1: 固化设计与项目入口

**Files:**
- Create: `README.md`
- Create: `项目首页.md`
- Create: `文档/设计与计划/设计说明/Harness-SDD工作流-v0.1设计说明.md`

- [x] **Step 1:** 记录背景、目标、非目标、资产模型、门禁和问题分级。
- [x] **Step 2:** 在 README 中提供目录导航、推荐结构和最小试点步骤。
- [x] **Step 3:** 检查设计不包含平台绑定或多仓库实现。

### Task 2: 提供可执行流程

**Files:**
- Create: `文档/工作流程/单需求单仓库SDD工作流-v0.1.md`

- [x] **Step 1:** 描述从原始需求到 Spec Approved 的分析和评审流程。
- [x] **Step 2:** 描述开发期间 L0、L1、L2 问题的处理回路。
- [x] **Step 3:** 描述 Code PR 的追溯和验收门禁。

### Task 3: 提供 Spec 与评审模板

**Files:**
- Create: `模板/规格文档/需求规格模板.md`
- Create: `模板/规格文档/技术设计模板.md`
- Create: `模板/规格文档/实施任务模板.md`
- Create: `模板/规格文档/决策记录模板.md`
- Create: `模板/评审清单/规格评审清单.md`
- Create: `模板/评审清单/代码评审一致性清单.md`

- [x] **Step 1:** 创建不包含具体业务信息的通用模板。
- [x] **Step 2:** 将业务、开发、测试的评审责任写入清单。
- [x] **Step 3:** 将 Spec 路径、基线提交和验收证据写入 Code PR 清单。

### Task 4: 验证并发布

**Files:**
- Verify: repository Markdown files

- [x] **Step 1:** 运行 `rg --files README.md 项目首页.md 文档 模板`，确认全部交付物存在。
- [x] **Step 2:** 运行 `rg -n "T[B]D|T[O]DO" README.md 项目首页.md 文档 模板`，确认没有未处理占位内容。
- [x] **Step 3:** 运行 `git diff --check`，确认没有空白错误。
- [x] **Step 4:** 检查 Git 差异只包含 v0.1 文档和模板。
- [ ] **Step 5:** 提交并推送 `main`。

## 相关笔记

- [[项目首页]]
- [[Harness-SDD工作流-v0.1设计说明]]
- [[单需求单仓库SDD工作流-v0.1]]
