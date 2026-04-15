# WEBSITE_RELEASE_PIPELINE__SSOT__v1

**产品化名称**：website_release_pipeline_v1

**阶段**：Design Freeze（v1）

## 1. 目标
把已在 `proj_p1_05_0001` 验证跑通的一次“网站发布闭环”（从站点展示层补齐 → Git 仓库 bootstrap → 部署适配 → deployment facts 回写 → 项目级 acceptance verdict → SSOT freeze → closeout）抽象为可复用的系统能力。

## 2. 边界（必须遵守）
1. OpenClaw 仍是唯一合法主系统，不引入第二主控。
2. 不修改 OpenClaw 源码。
3. 实现必须落在 `ai-plus / adapter / skill / governance` 层完成。
4. 不制造长期“双活维护模式”（例如同时维护 workspace 大仓 + git-projects 发布仓作为两个长期 SSOT）。
5. deployment facts、verdict、SSOT freeze 必须纳入标准流水线（不是散装手工步骤）。
6. 设计必须支持未来其它网站项目复用，不只服务单个 `proj_p1_05_0001`。
7. 必须遵循“先设计冻结、再分阶段落地”。

## 3. 输入 / 输出（端到端）
### 输入（Input）
- 项目分类信息（站点源码 / 静态展示目录 / 治理证据镜像仓库）
- 发布仓 bootstrap 需求（是否需复制到独立 git 仓库、remote、分支策略）
- 部署适配器选择（本 v1 以 Cloudflare 为第一适配器；接口预留其它适配器）
- 项目级 acceptance verdict 的证据链路径（reports / artifacts / evidence_index 等）

### 输出（Output）
- 发布仓具备最小 site shell（若原仓为治理/证据镜像仓库）
- deployment facts：
  - `public_url`
  - `deployment_status`
  - `deploy_target`
  - `repo_url`
  - `branch`
- 项目级 acceptance verdict：
  - complete / partial / failed / unknown
  - authoritative verdict 工件路径
- SSOT freeze：
  - 单一最终 SSOT 路径
  - 非 SSOT 副本明确降级/指向
- closeout 文档化

## 4. 阶段与主流程
**Stage 0：分类与门禁准备**
- 分类站点项目类型
- 决定走哪个发布路径（源码 / 静态展示 / 治理镜像补洞）

**Stage 1：独立发布仓 bootstrap（repo）**
- 避免 workspace 大仓直接发布
- 在独立目录执行 `git init`、本地 commit、分支迁移、remote + auth 预检

**Stage 2：最小 site shell 补齐（site）**
- 若为治理/证据镜像仓库：自动补最小 `public/index.html`
- 若为静态展示目录：确认直接可部署入口

**Stage 3：部署适配（deploy adapter）**
- Cloudflare Pages/Workers 适配器完成部署
- 收集 deployment facts

**Stage 4：deployment facts 自动写回（facts sync）**
- 将 facts 写回项目状态/证据链（受控字段集合）

**Stage 5：项目级 acceptance verdict（verdict engine）**
- 用项目级规则引擎把 evidences 推导为 overall_result
- 输出 authoritative verdict 工件

**Stage 6：SSOT freeze & replica demotion（SSOT）**
- 冻结单一 SSOT 路径
- 对非 SSOT 副本只保留历史信息并明确不再作为对账/裁决源

**Stage 7：closeout**
- 生成最终 closeout 文档
- 标注 final_status

## 5. 状态推进（对外语义）
- 阶段状态与 `execution_status / acceptance_status / overall_result` 对齐。
- 当且仅当 verdict = complete 且无其它治理阻塞时，允许执行状态推进。

## 6. 风险与 failure handling
- Deployment 失败：回滚到上一次可用 public_url（如适配器支持）或保持 partial，并记录原因。
- Evidence 不足：verdict 只能为 partial/unknown，不得伪造 complete。
- A/B 双副本：必须执行 SSOT freeze，避免后续漂移。
\n---\n
System-level canonical path: /home/openclaw/.openclaw/workspace/ai-plus/governance/ssot/docs/WEBSITE_RELEASE_PIPELINE__SSOT__v1.md
Role of this file: sample-trace (not system SSOT host).
