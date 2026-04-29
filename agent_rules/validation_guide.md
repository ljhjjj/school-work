# Validation Guide

## 1. 목적
작성 완료된 마크다운 노트와 PLAN 파일이 프로젝트 규칙을 준수하는지 검증한다.
본문 작성 후, PLAN 업데이트 후, 최종 리포트 출력 직전에 이 가이드를 적용한다.

## 2. 필수 입력
- 대상 마크다운 파일: `[과목명]/[과목명] [주차]-[차시].md`
- 대상 PLAN 파일: `[과목명]/[과목명]_[주차]-[차시]_PLAN.md`
- 검증 규칙: `schemas/validation_rules.json`
- 리포트 템플릿: `schemas/report_templates.json`
- 실행 정책: `schemas/codex_policy.json`

## 3. Required Checks
- `schemas/validation_rules.json`의 `paths`와 `encoding` 규칙을 확인한다.
- `markdown_structure` 규칙에 따라 H1, H2, H3 구조를 확인한다.
- `latex` 규칙에 따라 인라인/블록 수식, 금지 패턴, 경고 매크로를 확인한다.
- PLAN 파일과 출력 파일 경로가 일치하는지 확인한다.
- PLAN 체크박스의 완료/미완료 상태가 재개 지점과 일관되는지 확인한다.

## 4. 태그 분류 기준
- `normal_status_tags.search_terms`는 정상 판정/범위 관리 태그다. 발견되어도 validation warning으로 잡지 않는다.
- `residual_issue_tags.search_terms`는 미해결 태그다. 발견된 항목은 최종 리포트의 `[잔여 이슈]`에 포함한다.
- `warning_tags.search_terms`는 하위 호환용 별칭이며 `residual_issue_tags.search_terms`와 같은 기준으로만 사용한다.
- 정상 판정/범위 관리 태그를 `warning_tags.search_terms`에 추가하지 않는다.

## 5. 리포트 기준
- 정상 판정/범위 관리 태그는 필요하면 작업 요약이나 범위 기록에 남긴다.
- 잔여 이슈 태그는 누락하지 않고 최종 리포트의 `[잔여 이슈]`에 남긴다.
- `false_positive_exceptions`에 해당하는 리포트 제목이나 완료 처리 문구는 잔여 이슈로 세지 않는다.
