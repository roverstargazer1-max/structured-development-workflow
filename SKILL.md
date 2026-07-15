---
name: structured-development-workflow
description: 面向软件项目的开发规划与文档治理 Skill。仅在用户明确要求需求澄清、PRD、产品流程、页面草图、技术架构、数据模型、API 契约、影响分析、实施路线图、任务拆分、角色分工、执行交接、开发文档新增/删改/更新或外部实施结果同步时使用。读取仓库和代码作为事实依据，按 Level 1-3 生成并维护经批准的文档、任务卡和交接包；不编写或修改业务代码与测试，不安装依赖，不运行应用测试、构建、迁移或代码生成，不执行 Git 写操作、提交、推送、PR、合并、发布或部署。普通“实现功能”“修复 Bug”“重构代码”等仅要求实际编码而未要求规划或文档的请求不应隐式触发；显式调用 $structured-development-workflow 时，即使请求包含实现，也只能完成规划、文档和交接，不得进入代码执行。
---

# 结构化开发规划与文档治理

## 目标与边界

把模糊开发需求整理为可追踪、可批准、可分配、可交接的文档体系。读取代码和配置只是为了建立事实基础；输出重点是文档、任务和决策，不是代码。

允许在批准后创建或更新人类可读开发文档。允许在获得单独明确批准后维护 OpenAPI、Proto、Schema 等机器可读契约规范。禁止修改业务代码、测试、构建配置、依赖、数据库迁移和实现性脚本；禁止运行应用测试、构建、迁移、代码生成或其他实施命令；禁止执行 Git 写操作和远程操作。

默认使用用户使用的语言；用户使用中文时，用中文编写说明和文档。保持命令、路径、协议字段和代码标识符的原始形式。

## 引用导航

只读取当前阶段需要的引用：

- 判定 Level 1-3 和文档深度时，读取 [references/task-sizing.md](references/task-sizing.md)。
- 推进状态、需求变化或进度同步时，读取 [references/workflow-stages.md](references/workflow-stages.md)。
- 编写 PRD、流程、草图、架构、数据模型或 API 契约时，读取 [references/requirements-and-design-templates.md](references/requirements-and-design-templates.md)。
- 输出影响分析和实施路线图时，读取 [references/impact-and-roadmap-templates.md](references/impact-and-roadmap-templates.md)。
- 拆分任务、建议角色和生成执行交接包时，读取 [references/task-decomposition-and-handoff.md](references/task-decomposition-and-handoff.md)。
- 设计测试、验收或同步外部执行结果时，读取 [references/verification-and-result-sync.md](references/verification-and-result-sync.md)。
- 新增、更新、合并、归档、删除文档或修改契约时，读取 [references/document-governance-and-safety.md](references/document-governance-and-safety.md)。

## 主状态机

按以下状态推进，并在阶段更新中说明当前状态和下一门禁：

`DISCOVER → CLARIFY → SIZE → DOCUMENT_AUDIT → DESIGN → IMPACT_ANALYSIS → DOCUMENT_CHANGE_PLAN → WAITING_FOR_DOCUMENT_APPROVAL → WRITE_DOCUMENTS → TASK_DECOMPOSITION → ASSIGNMENT_AND_HANDOFF → DOCUMENT_VALIDATION → WAITING_FOR_USER_ACCEPTANCE → DONE`

允许按 Level 跳过不适用的设计产物，但禁止越过以下门禁：

1. 未读取足够项目上下文，不得确定设计和任务边界。
2. 存在阻塞性歧义，不得形成最终文档变更计划。
3. 未盘点已有文档、引用和仓库惯例，不得新建平行文档体系。
4. 未输出文档变更计划并得到明确批准，不得写入、移动、合并或归档文档。
5. 修改 OpenAPI、Proto、Schema 等机器可读契约前，必须取得针对具体契约和影响范围的单独批准。
6. 永久删除任何文档前，必须检查引用、提供替代或归档方案，并取得单独明确批准。
7. 未完成文档与契约验证，不得宣称文档交付完成。

## 工作流程

### 1. 读取事实上下文

先定位工作目录、仓库根目录和适用的 `AGENTS.md`。使用只读方式检查 Git 状态、历史与 diff，并有针对性地读取 README、文档索引、贡献指南、依赖清单、相关代码/测试、路由、Schema、迁移和接口规范。

把现有代码、配置、测试、文档和明确决策作为事实来源。只读取与当前规划相关的内容，不无目的遍历大型仓库。不得通过运行应用、测试或构建来代替静态调研。

### 2. 澄清意图并分级

复述目标、非目标、用户/角色、范围、约束和可验证验收标准。先从仓库寻找答案，仅询问无法可靠推断且会改变产品、架构、数据、接口、任务或文档决策的问题。

依据范围、风险、模块数量、数据/安全影响、外部契约、可逆性和不确定性划分 Level 1-3，并说明需要的文档产物和不需要的产物。

### 3. 盘点文档并形成设计

列出现有相关文档、状态、职责、引用、重复或过时项。优先更新权威文档；不要创建与现有 PRD、架构、Schema 或 API 规范冲突的平行来源。

按 Level 生成最少充分设计：Level 1 使用单份变更说明；Level 2 按需生成轻量 PRD、流程、技术/数据/API 说明和任务包；Level 3 形成完整适用文档链。

### 4. 分析影响并提出文档变更计划

说明产品、页面、模块、API、数据、安全、性能、一致性、可观测性、发布、回滚、测试和文档的适用影响。区分已确认事实、设计决策、待验证假设和执行阶段风险。

在对任何项目文档落盘前，输出新增、更新、合并、归档、拟删除和不变项清单，列出目标路径、原因、信息来源、引用影响和验证方式，然后进入 `WAITING_FOR_DOCUMENT_APPROVAL`：

> 以上是准备执行的文档变更计划。目前尚未修改项目文档或任何业务代码。请确认文档计划，或指出需要调整的部分。

### 5. 编写经批准的文档

批准后只编辑计划中列出的文档和契约规范。遵循仓库现有结构；没有约定时，Level 1 默认使用 `docs/plans/<task>.md`，Level 2-3 默认使用 `docs/planning/<task>/`。

在现有格式允许时记录文档状态、负责人、来源需求、关联决策、关联任务和更新时间。不要为了模板完整而虚构页面、服务、字段、接口或负责人。

### 6. 拆分、分配并交接任务

把设计转换为可独立执行和验收的任务卡，标明依赖、并行批次、阻塞条件、输入材料、允许/禁止范围、预期产物、完成定义、测试要求、回滚和文档同步要求。

未知具体人员时填写推荐角色并标记 `Unassigned`。生成可直接交给开发者或执行 Agent 的任务提示词，但不得启动、委派、协调或监督执行 Agent。

### 7. 验证文档并等待验收

只运行不会实施业务变更的文档链接、格式、引用、Mermaid、YAML/JSON 语法或契约校验。不得运行应用测试、构建、迁移、代码生成或修复业务实现。

汇总实际创建/更新/归档的文档、任务关系、验证结果、未决问题和执行交接入口，进入 `WAITING_FOR_USER_ACCEPTANCE`。用户验收后结束本轮文档工作；不得继续进入编码或 Git 流程。

## 进度同步入口

当用户提供开发者状态、测试结果、diff 摘要、验收反馈或发布结果并要求更新文档时，从 `PROGRESS_SYNC` 进入：

`PROGRESS_SYNC → DOCUMENT_AUDIT → DOCUMENT_CHANGE_PLAN → WAITING_FOR_DOCUMENT_APPROVAL → WRITE_DOCUMENTS → DOCUMENT_VALIDATION → WAITING_FOR_USER_ACCEPTANCE → DONE`

只记录有来源的结果，区分“执行方报告”“本 Skill 静态核对”和“尚未验证”。不得修复代码、补写测试、重跑应用检查或替执行方宣称成功。

## 安全与完成规则

- 只使用只读 Git 命令；禁止 `git add`、提交、推送、分支修改、PR、合并、标签和发布操作。
- 不安装依赖，不写临时实现脚本，不调用代码生成，不改变运行环境或数据库。
- 契约规范虽属于允许文档范围，但因可能驱动代码生成或兼容行为，始终使用额外批准门禁。
- 默认先标记废弃或迁移至仓库既有归档区；永久删除始终需要单独批准。
- 完成意味着文档、任务和交接包已获批准、落盘并通过文档级验证，不意味着任何业务功能已经实现。
