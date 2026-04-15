# WEBSITE_RELEASE_PIPELINE__MODULES_AND_INTERFACES__v1

## 模块总览
> 本 v1 把链路拆成独立可替换模块，支持复用与渐进式落地。

### 1) repo_publish_bootstrap_v1
- **是否纳入 v1**：✅ 是
- **理由**：避免 workspace 大仓直接发布；可复用于所有网站项目。
- **类型**：独立模块
- **输入**：
  - project_id
  - source_repo_path（可能为 workspace/或其它）
  - target_repo_path（git-projects 独立目录）
  - remote_url（可选：用于 auth 预检）
- **输出**：
  - 独立 git 仓库存在
  - branch = main
  - remote（如允许）存在

- **与 governance / state 的关系**：
  - 写入/更新“发布仓路径与分支”的 facts 字段

### 2) static_site_shell_bootstrap_v1
- **是否纳入 v1**：✅ 是（但作为子模块/子步骤）
- **理由**：治理/证据镜像仓库需要最小可部署入口。
- **类型**：子模块（挂在 Stage 2）
- **输入**：项目类型（治理镜像/静态展示/站点源码）
- **输出**：
  - public/index.html 或其它最小入口

### 3) cloudflare_deploy_adapter_v1
- **是否纳入 v1**：✅ 是（首适配器）
- **理由**：本样本链路由 Cloudflare 触发，需标准接口。
- **类型**：独立模块
- **输入**：
  - repo_url
  - branch
  - site entry / output dir（如 public）
- **输出**：
  - deployment_status
  - public_url
  - deploy_target

### 4) deployment_facts_sync_v1
- **是否纳入 v1**：✅ 是
- **理由**：deployment facts 必须进入可审计证据链，而不是口头确认。
- **类型**：独立模块
- **输入**：
  - deployment facts
  - 写回字段白名单（受控字段集合）
- **输出**：
  - current_state.json / state 文档的受控字段更新

### 5) project_acceptance_verdict_engine_v1
- **是否纳入 v1**：✅ 是
- **理由**：verdict 是系统能力核心（不能散装规则）。
- **类型**：独立模块
- **输入**：
  - evidence_index / proofs / reports
  - complete criteria / verdict rule 工件
- **输出**：
  - authoritative verdict 工件
  - acceptance.json overall_result（complete/partial/failed/unknown）

### 6) project_ssot_freeze_and_replica_demotion_v1
- **是否纳入 v1**：✅ 是
- **理由**：A/B 双副本漂移是系统级缺陷来源。
- **类型**：独立模块
- **输入**：
  - SSOT 候选（通常来自 deployment 事实与 verdict 完成态）
  - replica 列表
- **输出**：
  - SSOT freeze 文档/状态更新
  - 非 SSOT 副本降级指向

## 模块间接口契约（简化）
- **Stage 1 → Stage 2**：repo_publish_bootstrap 输出 repo 基础信息
- **Stage 2 → Stage 3**：static_site_shell 输出 site entry 与目录约定
- **Stage 3 → Stage 4**：deploy adapter 输出 deployment facts
- **Stage 4 → Stage 5**：facts sync 写回 state
- **Stage 5 → Stage 6**：verdict engine 输出 overall_result=complete/partial
- **Stage 6 → Stage 7**：ssot freeze 生成 final closeout
