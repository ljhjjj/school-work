# Structure and LaTeX Validation

## [P1] 3. Required Checks

- 구조 검증 항목은 `schemas/validation_rules.json`의 `markdown_structure`, `latex`, `input_coverage`를 따른다.
- 파일 경로 및 인코딩 규칙은 해당 JSON의 `paths`, `encoding`을 따른다.
- 태그 분류 및 리포트 반영 기준은 `agent_rules/validation/report_validation.md`를 따른다.
- Markdown 가이드는 검증 절차와 보고 방식만 설명한다.
- `latex` 규칙에 따라 인라인/블록 수식, 금지 패턴(`<math>` 태그 등), deprecated 매크로를 확인한다.
- `\begin{aligned}`, `\begin{array}`, `\underbrace`, `\overbrace`는 표준 매크로이므로 warning 대상이 아니다.
- `\begin{eqnarray}` 등 deprecated 매크로는 `report_only`로 기록한다.

## [P0/P1] 3.1 Hard Rule / Soft Rule

- Hard/soft rule의 실제 분류는 `schemas/validation_rules.json`의 `severity.hard_rules`와 `severity.soft_rules`를 따른다.
- 이 섹션의 예시는 해당 JSON 분류를 설명하기 위한 보조 설명이다.
- Hard rule 위반은 작업 완료로 인정하지 않는다. 가능한 경우 자동 수정하고, 자동 수정이 불가능하면 최종 리포트의 잔여 이슈에 명시한다.
- Soft rule 위반은 작업을 중단하지 않는다. 최종 리포트 또는 PLAN의 `[⚠️ 실행 이슈]` / `잔여 이슈`에 기록한다.
- 제목이 존재하지 않는 경우와 권장 제목 형식에 맞지 않는 경우를 구분한다.
- H1 required count 위반은 hard rule이다.
- H1/H2/H3가 `recommended_pattern`에 맞지 않는 경우는 soft rule이다.
- 자유 제목은 허용하되, 강의노트 표준 형식에서는 `recommended_pattern`을 권장한다.
- H2 구분선 누락은 문서 구조 일관성을 위해 hard rule로 유지한다.
