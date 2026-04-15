# WEBSITE_RELEASE_PIPELINE__IMPLEMENTATION_PLAN__v1

> 分阶段落地：先把“可复用设计+最小可实现接口”固定下来，再逐步接入自动化。

## Phase 1（最小可落地）
目标：实现可重复的设计-执行闭环（但不做大规模系统级重构）。

- Step 1：补齐/统一 site shell 的最小策略（static_site_shell_bootstrap_v1）
- Step 2：独立 repo bootstrap 的标准流程（repo_publish_bootstrap_v1）
- Step 3：部署适配器 v1（cloudflare_deploy_adapter_v1）
- Step 4：deployment facts sync 受控写回（deployment_facts_sync_v1）
- Step 5：项目级 verdict engine v1（project_acceptance_verdict_engine_v1）
- Step 6：SSOT freeze（project_ssot_freeze_and_replica_demotion_v1）

验收（Acceptance）
- 对样本 `proj_p1_05_0001`：能复现 complete/partial 的裁决与 SSOT freeze。

## Phase 2（增强）
- 增加更多 site shell 模板
- 提供多种静态入口发现策略
- 引入可选的“站点内容快照”以增强审计性

## 回滚边界
- 禁止改变 verdict 输出的证据来源
- 禁止跳过 facts sync
\n---\n
System-level canonical path: /home/openclaw/.openclaw/workspace/ai-plus/governance/ssot/docs/WEBSITE_RELEASE_PIPELINE__IMPLEMENTATION_PLAN__v1.md
Role of this file: sample-trace (not system SSOT host).
