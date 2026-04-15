# WEBSITE_RELEASE_PIPELINE__PROMPT_CONTRACTS__v1

> 这里给出面向 future bot 的调用模板：标准输入 schema、输出 schema、以及动作边界。

## 0) 输入字段 Schema（统一）
- `project_id`：string
- `project_type`："source_site" | "static_site_dir" | "evidence_mirror_repo"
- `repo_source`：string（workspace大仓或其它）
- `repo_target`：string（独立发布仓目录）
- `deploy_adapter`："cloudflare"（v1）
- `evidence_paths`：string[]（reports/artifacts/evidence_index 等）

## 1) 只读预检模板（Read-only preflight）
### Prompt
- “仅仅读取项目状态/证据/规则；输出 classification、可部署入口可得性、需要的最小 site shell 类型，以及是否具备 facts sync 与 verdict 所需的输入证据。”

### Output Schema
- `classification`：project_type
- `shell_needed`：boolean
- `required_inputs_missing`：string[]
- `ssot_candidate`：string（可选）

## 2) 执行发布模板（Execute release）
### Prompt
- “在独立发布仓执行 bootstrap；补齐最小 site shell；调用 deployment adapter；写回 deployment facts（受控字段）；生成 authoritative verdict；冻结 SSOT；输出 closeout 路径。”

### Output Schema
- `deployment_facts`：{public_url,deployment_status,deploy_target,repo_url,branch}
- `authoritative_verdict_path`
- `acceptance_overall_result`
- `ssot_final_path`
- `closeout_path`

## 3) closeout / ssot freeze 模板
### Prompt
- “基于 verdict 与 deployment facts，写入 SSOT freeze 文档；对非 SSOT 副本降级指向；生成 final closeout 文档并标注 final_status=COMPLETED（或 partial 并列 remaining blockers）。输出 closeout 路径与后续读取优先级。”

### Output Schema
- `final_status`：COMPLETED | PARTIAL
- `final_ssot_path`
- `non_ssot_replica_actions`：string[]
- `remaining_blockers`：string[]
