# WEBSITE_RELEASE_PIPELINE__STATE_MACHINE__v1

> 这是 v1 的状态机设计冻结：规定每阶段状态迁移必须满足哪些可验证条件。

## 统一状态空间
- `project_type`：source_site | static_site_dir | evidence_mirror_repo
- `repo_state`：bootstrap_needed | bootstrapped
- `site_shell_state`：missing_shell | shell_ready
- `deploy_state`：not_deployed | deploying | deployed
- `facts_state`：not_synced | synced
- `verdict_state`：not_computed | computed
- `ssot_state`：not_frozen | frozen
- `closeout_state`：not_done | done

## 阶段迁移规则
### 0) classification
- 输入分类识别完成 → 进入 Stage 1
- 失败：标记 unknown，停止或进入最小静态补齐路径（依据门禁策略）

### 1) repo bootstrap
- 当 target repo 存在且分支 main 就绪 → 进入 Stage 2
- 失败：停止并保留证据（否则后续不可审计）

### 2) site shell
- 当存在最小入口（public/index.html 或等价） → 进入 Stage 3

### 3) deploy
- adapter 返回 public_url 且 deployment_status = active/ready → 进入 Stage 4

### 4) facts sync
- 写回受控字段成功 → 进入 Stage 5
- 若写回失败：不得继续 verdict（否则会读到错误/缺失事实）

### 5) verdict
- 使用 verdict rule 工件进行裁决
- overall_result=complete → 允许进入 Stage 6
- overall_result=partial/failed/unknown → 允许进入 Stage 6（但执行态保持阻塞），并在 closeout 标注 remaining blockers

### 6) ssot freeze
- 当 verdict 输出完成且 deploy facts 已存在 → 冻结 SSOT
- 非 SSOT 副本必须明确降级指向

### 7) closeout
- 生成 final closeout 文档

## 与 acceptance / execution 的状态机约束
- 当 acceptance.overall_result != complete：execution_status 必须保持阻塞/未完成态。
- 禁止把 evidence 或 proof 的 partial 成功直接当成 project-level complete。
\n---\n
System-level canonical path: /home/openclaw/.openclaw/workspace/ai-plus/governance/ssot/docs/WEBSITE_RELEASE_PIPELINE__STATE_MACHINE__v1.md
Role of this file: sample-trace (not system SSOT host).
