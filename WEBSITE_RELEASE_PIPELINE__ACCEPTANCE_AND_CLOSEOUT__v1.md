# WEBSITE_RELEASE_PIPELINE__ACCEPTANCE_AND_CLOSEOUT__v1

## overall_result 枚举定义
- **complete**：满足项目级 complete criteria，且 deployment facts 与 evidence 链可对账。
- **partial**：证据链足以支持部分裁决（例如 proof passed）但缺少 complete 的权威裁决信号。
- **failed**：关键证据/事实与 complete criteria 明确冲突。
- **unknown**：证据/规则不足以做裁决，或缺少 authoritative source 绑定。

## deployment facts 如何进入 verdict
- deployment facts 由 adapter 输出 → facts sync 写回 current_state
- verdict engine 仅读取 current_state 的受控字段（public_url、deployment_status、repo_url、branch、deploy_target 等）
- 禁止通过口头信息影响 verdict。

## SSOT freeze 与 closeout 触发条件
- SSOT freeze：当 verdict_state=computed 且 deploy_state=deployed（有 public_url）时触发。
- closeout：当 ssot_state=frozen 且最终 remaining blockers 已在文档化中列出。
\n---\n
System-level canonical path: /home/openclaw/.openclaw/workspace/ai-plus/governance/ssot/docs/WEBSITE_RELEASE_PIPELINE__ACCEPTANCE_AND_CLOSEOUT__v1.md
Role of this file: sample-trace (not system SSOT host).
