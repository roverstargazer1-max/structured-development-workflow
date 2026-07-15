---
name: structured-development-workflow
description: 面向软件项目的开发规划与文档治理 Skill。仅在用户明确要求需求澄清、PRD、产品流程、页面草图、技术架构、数据模型、API 契约、影响分析、实施路线图、任务拆分、角色分工、执行交接、开发文档新增/删改/更新或外部实施结果同步时使用。读取仓库和代码只为核实事实；若真实背景不足以支撑完整可信的文档，必须停止下游推进并分轮询问。信息充分后，按 Level 1-3 创建或维护有索引、编号、交叉引用和任务卡的持久化文档包；不得用聊天中的长段总结替代正式文档。不编写或修改业务代码与测试，不安装依赖，不运行应用测试、构建、迁移或代码生成，不执行 Git 写操作、提交、推送、PR、合并、发布或部署。普通“实现功能”“修复 Bug”“重构代码”等仅要求实际编码而未要求规划或文档的请求不应隐式触发；显式调用 $structured-development-workflow 时，即使请求包含实现，也只能完成规划、文档和交接，不得进入代码执行。
---

# 结构化开发规划与文档治理

## 目标与边界

把模糊开发需求整理为可追踪、可批准、可分配、可交接的持久化文档体系。主要产物是仓库内有索引和交叉引用的文档、任务卡、角色建议和执行交接包；聊天只用于澄清、审批和交付导航。读取代码和配置只是为了核实可发现事实，不能替代用户提供真实业务背景、意图和取舍；输出重点是文档与任务治理，不是代码实现。

允许在批准后创建或更新人类可读开发文档。允许在获得单独明确批准后维护 OpenAPI、Proto、Schema 等机器可读契约规范。禁止修改业务代码、测试、构建配置、依赖、数据库迁移和实现性脚本；禁止运行应用测试、构建、迁移、代码生成或其他实施命令；禁止执行 Git 写操作和远程操作。

默认使用用户使用的语言；用户使用中文时，用中文编写说明和文档。保持命令、路径、协议字段和代码标识符的原始形式。

## 引用导航

只读取当前阶段需要的引用：

- 判定 Level 1-3 和文档深度时，读取 [references/task-sizing.md](references/task-sizing.md)。
- 规划文档颗粒度、包级索引、元数据、编号、追踪和最终交付时，读取 [references/document-package-and-traceability.md](references/document-package-and-traceability.md)。
- 检查文档就绪度、分轮澄清、推进状态、需求变化或进度同步时，读取 [references/workflow-stages.md](references/workflow-stages.md)。
- 编写 PRD、流程、草图、架构、数据模型或 API 契约时，读取 [references/requirements-and-design-templates.md](references/requirements-and-design-templates.md)。
- 输出影响分析和实施路线图时，读取 [references/impact-and-roadmap-templates.md](references/impact-and-roadmap-templates.md)。
- 拆分任务、建议角色和生成执行交接包时，读取 [references/task-decomposition-and-handoff.md](references/task-decomposition-and-handoff.md)。
- 设计测试、验收或同步外部执行结果时，读取 [references/verification-and-result-sync.md](references/verification-and-result-sync.md)。
- 新增、更新、合并、归档、删除文档或修改契约时，读取 [references/document-governance-and-safety.md](references/document-governance-and-safety.md)。

## 主状态机

按以下状态推进，并在阶段更新中说明当前状态和下一门禁：

`DISCOVER → CLARIFY ↔ WAITING_FOR_REQUIRED_CONTEXT → SIZE → DOCUMENT_AUDIT → DESIGN → IMPACT_ANALYSIS → DOCUMENT_CHANGE_PLAN → WAITING_FOR_DOCUMENT_APPROVAL → WRITE_DOCUMENTS → TASK_DECOMPOSITION → ASSIGNMENT_AND_HANDOFF → DOCUMENT_VALIDATION → WAITING_FOR_USER_ACCEPTANCE → DONE`

允许按 Level 跳过不适用的设计产物，但禁止越过以下门禁：

1. 未读取足够项目上下文，不得确定设计和任务边界。
2. 未通过文档就绪检查时，必须进入 `WAITING_FOR_REQUIRED_CONTEXT`；只允许输出已确认事实、缺失信息、阻塞原因和待回答问题，不得分级、设计、输出文档目录或正文草案、形成文档变更计划或拆分任务。仅当用户明确把本轮交付限定为问题清单、调研提纲、决策待办或纯目录建议时，才可输出对应辅助材料并声明其不是正式文档。
3. 未盘点已有文档、引用和仓库惯例，不得新建平行文档体系。
4. 未输出文档变更计划并得到明确批准，不得写入、移动、合并或归档文档。
5. 修改 OpenAPI、Proto、Schema 等机器可读契约前，必须取得针对具体契约和影响范围的单独批准。
6. 永久删除任何文档前，必须检查引用、提供替代或归档方案，并取得单独明确批准。
7. 未完成文档与契约验证，不得宣称文档交付完成。
8. Level 2-3 未形成可发现的文档包索引、适用的独立文档和任务入口时，不得用聊天总结或单个“大而全”文件替代交付。

## 工作流程

### 1. 读取事实上下文

先定位工作目录、仓库根目录和适用的 `AGENTS.md`。使用只读方式检查 Git 状态、历史与 diff，并有针对性地读取 README、文档索引、贡献指南、依赖清单、相关代码/测试、路由、Schema、迁移和接口规范。

把现有代码、配置、测试、文档和明确决策作为事实来源。只读取与当前规划相关的内容，不无目的遍历大型仓库。不得通过运行应用、测试或构建来代替静态调研。

### 2. 澄清意图并检查文档就绪度

复述文档目的与受众、真实背景与当前问题、目标与非目标、用户/角色、范围、约束、关键业务规则、验收标准和权威资料来源。先从仓库寻找可发现答案，仅询问无法可靠推断且会改变需求含义、架构、数据、接口、安全、验收、任务或文档决策的问题。

按 `references/workflow-stages.md` 执行通用和文档类型就绪检查。存在阻塞项时，每轮只提出 1-3 个最高优先级问题，然后进入 `WAITING_FOR_REQUIRED_CONTEXT`；用户回答后重新检查，未通过时继续澄清。不得用 `Assumption`、`Unknown`、模板占位符或较高 Level 绕过门禁。

只有通过文档就绪检查后，才依据范围、风险、模块数量、数据/安全影响、外部契约、可逆性和不确定性划分 Level 1-3，并说明需要和不需要的文档产物。

### 3. 盘点文档并形成设计

列出现有相关文档、状态、职责、引用、重复或过时项。优先更新权威文档；不要创建与现有 PRD、架构、Schema 或 API 规范冲突的平行来源。

按 Level 生成最少充分文档包：Level 1 使用单份结构化变更说明；Level 2 建立权威索引并按职责生成适用的需求、设计、影响、验证和任务文档；Level 3 建立索引、完整适用文档链、独立决策记录和任务卡。不同受众、维护角色、审批范围或生命周期的内容必须分文件，不得合并成长段总结。

### 4. 分析影响并提出文档变更计划

说明产品、页面、模块、API、数据、安全、性能、一致性、可观测性、发布、回滚、测试和文档的适用影响。区分已确认事实、设计决策、不会改变已确认需求含义的执行阶段假设和风险；不得把缺失的产品事实降格为假设继续推进。

在对任何项目文档落盘前，输出文件级文档包清单，逐项列出文档 ID、职责、目标路径、操作、权威范围、上下游关系、信息来源、引用影响和验证方式，然后进入 `WAITING_FOR_DOCUMENT_APPROVAL`：

> 以上是准备执行的文档变更计划。目前尚未修改项目文档或任何业务代码。请确认文档计划，或指出需要调整的部分。

### 5. 编写经批准的文档

批准后只编辑计划中列出的文档和契约规范。遵循仓库现有结构；没有约定时，Level 1 默认使用 `docs/plans/<task>.md`，Level 2-3 按 `references/document-package-and-traceability.md` 建立 `docs/planning/<task>/` 文档包。

优先沿用仓库元数据规范；没有约定时，Level 2-3 必须记录文档 ID、状态、权威范围、负责人、推荐维护角色、来源、上下游关系、替代关系和更新时间。不要为了模板完整而虚构页面、服务、字段、接口或负责人。

### 6. 拆分、分配并交接任务

只把已批准且决策完整的设计转换为可独立执行和验收的任务卡，标明依赖、并行批次、阻塞条件、输入材料、允许/禁止范围、预期产物、完成定义、测试要求、回滚和文档同步要求。若仍缺少会改变任务目标、边界、依赖或完成定义的信息，返回 `CLARIFY`，不得生成任务卡。

未知具体人员时填写推荐角色并标记 `Unassigned`。生成可直接交给开发者或执行 Agent 的任务提示词，但不得启动、委派、协调或监督执行 Agent。

### 7. 验证文档并等待验收

只运行不会实施业务变更的文档链接、格式、引用、Mermaid、YAML/JSON 语法或契约校验。不得运行应用测试、构建、迁移、代码生成或修复业务实现。

以文档链接清单汇报实际创建、更新和归档项，再列出任务状态、验证结果、未决问题和执行交接入口；不要在聊天中重复粘贴全部正文。进入 `WAITING_FOR_USER_ACCEPTANCE`，用户验收后结束本轮文档工作；不得继续进入编码或 Git 流程。

## 进度同步入口

当用户提供开发者状态、测试结果、diff 摘要、验收反馈或发布结果并要求更新文档时，从 `PROGRESS_SYNC` 进入：

`PROGRESS_SYNC → DISCOVER → CLARIFY ↔ WAITING_FOR_REQUIRED_CONTEXT → DOCUMENT_AUDIT → DOCUMENT_CHANGE_PLAN → WAITING_FOR_DOCUMENT_APPROVAL → WRITE_DOCUMENTS → DOCUMENT_VALIDATION → WAITING_FOR_USER_ACCEPTANCE → DONE`

只记录有来源的结果，区分“执行方报告”“本 Skill 静态核对”和“尚未验证”。不得修复代码、补写测试、重跑应用检查或替执行方宣称成功。

## 安全与完成规则

- 只使用只读 Git 命令；禁止 `git add`、提交、推送、分支修改、PR、合并、标签和发布操作。
- 不安装依赖，不写临时实现脚本，不调用代码生成，不改变运行环境或数据库。
- 契约规范虽属于允许文档范围，但因可能驱动代码生成或兼容行为，始终使用额外批准门禁。
- 默认先标记废弃或迁移至仓库既有归档区；永久删除始终需要单独批准。
- 完成意味着文档、任务和交接包已获批准、按文档包清单落盘、可从索引发现并通过文档级验证，不意味着任何业务功能已经实现。
