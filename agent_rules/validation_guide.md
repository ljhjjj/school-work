<!-- Guide Tier: Mixed -->
<!-- Contains: P0 safety rules, P1 workflow rules, P2 reference rules -->

# Validation Guide

> 이 파일은 validation 하위 파일의 인덱스이다.
> 구체적인 검증 규칙은 아래 파일을 참조한다.

## 하위 파일 참조
- `agent_rules/validation/validation_overview.md`
- `agent_rules/validation/structure_and_latex_validation.md`
- `agent_rules/validation/input_coverage_validation.md`
- `agent_rules/validation/report_validation.md`

## Schema Canonical Source
- `agent_rules/schemas/validation_rules.json`
- `agent_rules/schemas/report_templates.json`

## 핵심 원칙
- Hard rule 위반은 작업 완료로 인정하지 않는다.
- Soft rule 위반은 작업을 중단하지 않으며 `completed_with_warnings` 등으로 보고할 수 있다.
