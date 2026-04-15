# evidence_interface_verification_summary

- artifact_id: P2M02-ARTIFACT-INDEX-001
- artifact_index_has_artifact: True
- pointer_target_matches_artifact_index_path: True
- verification_passed: True

## Repro steps
1) jq/equivalent JSON parse on schema/snapshot/index/pointer
2) Match artifact_id against artifact_index.entries
3) Compare pointer.target_canonical_path vs artifact_index.json path
