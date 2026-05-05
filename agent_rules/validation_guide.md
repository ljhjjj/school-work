<!-- Guide Tier: Mixed -->
<!-- Contains: P0 safety rules, P1 workflow rules, P2 reference rules -->

# Validation Guide

이 문서는 검증 가이드의 상위 인덱스다. 작성 완료 후, PLAN 업데이트 후, 최종 리포트 출력 전에 이 문서로 검증 원칙과 하위 파일 위치를 확인하고, 세부 기준은 전용 파일과 schema를 따른다.

## [P1] 1. 핵심 원칙

- 검증은 작성 완료 후, PLAN 업데이트 후, 최종 리포트 출력 전에 적용한다.
- 기계적 검증 규칙의 canonical source는 `agent_rules/schemas/validation_rules.json`이다.
- 최종 리포트 템플릿의 canonical source는 `agent_rules/schemas/report_templates.json`이다.
- Hard rule 위반과 Soft rule 위반은 구분한다.
- Hard rule 위반은 작업 완료로 인정하지 않는다.
- Soft rule 위반은 작업을 중단하지 않으며 `completed_with_warnings` 등으로 보고할 수 있다.

## [P1] 2. 하위 파일 참조

- 검증 흐름 개요와 전체 적용 순서: `agent_rules/validation/validation_overview.md`
- 구조 검증과 LaTeX 검증 기준: `agent_rules/validation/structure_and_latex_validation.md`
- 입력 범위와 누락 여부 검증 기준: `agent_rules/validation/input_coverage_validation.md`
- 리포트 기준과 validation_report 작성 규칙: `agent_rules/validation/report_validation.md`
- 기계적 검증 규칙 schema: `agent_rules/schemas/validation_rules.json`
- 최종 리포트 템플릿 schema: `agent_rules/schemas/report_templates.json`

## [P1] 3. 읽는 시점

- 검증 단계를 시작하거나 전체 순서를 다시 확인해야 하면 `agent_rules/validation/validation_overview.md`를 읽는다.
- 문서 구조, 헤더, 목록, 수식, LaTeX 표기를 점검해야 하면 `agent_rules/validation/structure_and_latex_validation.md`를 읽는다.
- 입력 범위 누락, 커버리지, 본문 포함 근거를 점검해야 하면 `agent_rules/validation/input_coverage_validation.md`를 읽는다.
- 환경 이슈, 잔여 이슈, validation_report 형식을 판단해야 하면 `agent_rules/validation/report_validation.md`를 읽는다.
- 체크 항목의 기계적 기준이 필요하면 `agent_rules/schemas/validation_rules.json`을 우선 확인한다.
- 최종 리포트 섹션과 상태 템플릿이 필요하면 `agent_rules/schemas/report_templates.json`을 우선 확인한다.
