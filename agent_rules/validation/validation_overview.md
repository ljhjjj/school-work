# Validation Overview

## [P2] 1. 목적
작성 완료된 마크다운 노트와 PLAN 파일이 프로젝트 규칙을 준수하는지 검증한다.
본문 작성 후, PLAN 업데이트 후, 최종 리포트 출력 직전에 이 가이드를 적용한다.

## [P1] 2. 필수 입력
- 대상 마크다운 파일: `output/[과목명]/[과목명] [주차]-[차시].md`
- 대상 PLAN 파일: `output/[과목명]/[과목명]_[주차]-[차시]_PLAN.md`
- 검증 규칙: `schemas/validation_rules.json`
- 리포트 템플릿: `schemas/report_templates.json`
- 실행 정책: `schemas/codex_policy.json`

## [P1] 3. 검증 순서 요약

1. `agent_rules/validation/structure_and_latex_validation.md`에 따라 구조 및 LaTeX 검증을 수행한다.
2. 입력 커버리지 확인이 필요한 작업은 `agent_rules/validation/input_coverage_validation.md`를 따른다.
3. 태그 분류, hard/soft 결과 보고, 최종 리포트 형식은 `agent_rules/validation/report_validation.md`를 따른다.
4. 기계적 검증 기준은 `agent_rules/schemas/validation_rules.json`을 우선한다.
5. 최종 리포트 템플릿은 `agent_rules/schemas/report_templates.json`을 우선한다.


자세한 구조 검증 및 LaTeX 검증 규칙은 `agent_rules/validation/structure_and_latex_validation.md`를 참조한다.

입력 커버리지 검증 규칙은 `agent_rules/validation/input_coverage_validation.md`를 참조한다.

태그 분류 및 리포트 기준은 `agent_rules/validation/report_validation.md`를 참조한다.
