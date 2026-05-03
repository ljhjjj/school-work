<!-- Guide Tier: Mixed -->
<!-- Contains: P0 safety rules, P1 workflow rules, P2 reference rules -->

# Validation Guide

## [P2] 1. 목적
작성 완료된 마크다운 노트와 PLAN 파일이 프로젝트 규칙을 준수하는지 검증한다.
본문 작성 후, PLAN 업데이트 후, 최종 리포트 출력 직전에 이 가이드를 적용한다.

## [P1] 2. 필수 입력
- 대상 마크다운 파일: `output/[과목명]/[과목명] [주차]-[차시].md`
- 대상 PLAN 파일: `output/[과목명]/[과목명]_[주차]-[차시]_PLAN.md`
- 검증 규칙: `schemas/validation_rules.json`
- 리포트 템플릿: `schemas/report_templates.json`
- 실행 정책: `schemas/codex_policy.json`

## [P1] 3. Required Checks

- 구조 검증 항목은 `schemas/validation_rules.json`의 `markdown_structure`, `latex`, `input_coverage`를 따른다.
- 파일 경로 및 인코딩 규칙은 해당 JSON의 `paths`, `encoding`을 따른다.
- 태그 분류 기준은 해당 JSON의 `normal_status_tags`와 `residual_issue_tags`를 따른다.
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

## [P1] 4. 입력 커버리지 검증
- Full PLAN에서는 입력 커버리지 검증을 H1/H2/H3, 수식, 경로 같은 구조 검증과 별도로 수행한다.
- Mini PLAN에서는 입력 커버리지 검증을 구조 검증 결과 안에 간단히 포함할 수 있다.
- Mini PLAN에서는 입력 커버리지 검증을 구조 검증과 통합하여 수행한다.
- Full PLAN에서는 구조 검증과 입력 커버리지 검증을 분리하여 수행한다.
- slide-only 작업에서 Mini PLAN을 사용하는 경우 전체 슬라이드 검토 여부만 확인한다.
- PLAN의 `페이지 판독 기록`과 최종 마크다운 본문을 대조해 입력별 핵심 항목 반영 여부를 확인한다.
- 손글씨 PDF, 슬라이드 PDF, STT TXT 등 입력 자료별로 확인한 핵심 개념, 공식, 도식, 용어가 본문에 반영되었거나 제외 사유가 기록되었는지 확인한다.
- 입력 자료가 여러 페이지이면 파일 단위로 뭉뚱그리지 않고 `p.1`, `p.2`처럼 페이지 단위 체크리스트를 만든다.
- 입력 자료의 페이지별 핵심 항목이 최종 노트에 반영되었는지 확인한다.
- 특정 페이지, 특정 파일명, 특정 용어는 manifest 또는 작업 PLAN에 명시된 경우에만 필수 검증 대상으로 삼는다.
- 예시: `handwritten_note.pdf p.3`, `reversible/reversed process`, 특정 도식 논증처럼 개별 차시에만 등장하는 항목은 범용 필수 규칙이 아니라 해당 차시 PLAN에 명시된 경우의 진단용 체크 항목이다.
- 슬라이드 PDF가 `verification_only` 또는 보조 확인용이면 본문 확장 여부가 아니라 용어·공식 확인 목적이 지켜졌는지 확인한다.
- Mini PLAN에서는 slide-only 작업의 전체 슬라이드 검토 여부를 간단히 확인하고, 필요한 경우 구조 검증 결과에 함께 기록한다.
- Full PLAN에서 `lecture_slide.pdf`만 제공되어 role이 `primary_scope_anchor`이고 scope가 `entire_pdf`인 경우에는 다음 항목을 필수 검증 대상으로 삼는다.  
  - `lecture_slide.pdf` 전체 페이지가 검토되었는지 확인한다.  
  - 각 슬라이드 또는 슬라이드 구간이 `페이지 판독 기록`에 포함되어 있는지 확인한다.  
  - 최종 본문에 슬라이드의 제목, 섹션 순서, 공식, 그림, 예제 흐름이 반영되었는지 확인한다.  
  - 본문에서 제외한 슬라이드가 있으면 PLAN 또는 최종 리포트에 제외 사유가 기록되었는지 확인한다.  
  - 외부 자료가 본문 범위 확장용으로 사용되지 않고 용어·공식 표기 확인용으로 제한되었는지 확인한다.
- STT TXT가 있는 경우 본문 흐름이 STT의 설명 범위와 충돌하지 않는지 확인한다.
- Full PLAN에서 구조 검증이 통과해도 입력 커버리지 검증이 미완료이면 작업 완료로 판정하지 않는다.
- Full PLAN에서는 PLAN 또는 최종 리포트에 `구조 검증 결과`와 `입력 커버리지 검증 결과`를 분리해 기록한다.

## [P1] 5. 태그 분류 기준
- `normal_status_tags.search_terms`는 정상 판정/범위 관리 태그다. 발견되어도 validation warning으로 잡지 않는다.
- `residual_issue_tags.search_terms`는 미해결 태그다. 발견된 항목은 최종 리포트의 `잔여 이슈`에 포함한다.
- `warning_tags.search_terms`는 하위 호환용 별칭이며 `residual_issue_tags.search_terms`와 같은 기준으로만 사용한다.
- 정상 판정/범위 관리 태그를 `warning_tags.search_terms`에 추가하지 않는다.

## [P1] 6. 리포트 기준
- 정상 판정/범위 관리 태그는 필요하면 작업 요약이나 범위 기록에 남긴다.
- 잔여 이슈 태그는 누락하지 않고 최종 리포트의 `잔여 이슈`에 남긴다.
- `false_positive_exceptions`에 해당하는 리포트 제목이나 완료 처리 문구는 잔여 이슈로 세지 않는다.
- PowerShell profile 권한 오류, 콘솔 인코딩 깨짐, Git 한글 경로 escape 표시 등은 산출물 검증 실패와 분리하여 `환경 이슈`로 기록한다.
- 환경 이슈가 실제 파일 저장, 검증 명령 실행, 산출물 품질을 막지 않았다면 작업 실패로 판정하지 않는다.
- 파일 자체가 UTF-8로 정상 디코딩되지만 터미널에서만 한글이 깨진 경우, 이를 파일 수정 이슈가 아니라 콘솔 출력 환경 이슈로 기록한다.
- 파일 자체에 깨진 한글 문자열이 남아 있거나 UTF-8 디코딩에 실패한 경우에만 파일 인코딩/본문 수정 이슈로 기록한다.
- `py --version` 실패나 WindowsApps Python 런처 오류는 PDF/OCR 작업 실패가 아니라 Python 실행 환경 이슈로 기록한다.
- `conda run -n pdfkit310 python`까지 실패한 경우에만 Python 기반 PDF/OCR 실행 가능성에 영향을 주는 환경 이슈로 승격한다.
- 환경 이슈가 없으면 `environment_issues` placeholder 값을 `없음`으로 채워 기록한다.
- 검증 결과는 hard rule 위반과 soft rule 위반으로 나누어 보고한다.
- hard rule 위반이 남아 있으면 `completed`로 처리하지 않는다.
- soft rule 위반만 남아 있으면 `completed_with_warnings` 또는 이에 준하는 상태로 보고할 수 있다.
- 최종 리포트는 `schemas/report_templates.json`의 `template_selection`에 따라 선택한다.
- Markdown 가이드에는 개별 템플릿 선택 규칙을 반복 서술하지 않는다.
- Mini PLAN에서는 입력 커버리지 검증 결과를 구조 검증 결과 안에 간단히 포함할 수 있다.
- Full PLAN에서 구조 및 입력 커버리지 검증 결과를 자세히 보고해야 하는 경우에는 `validation_report` 섹션을 함께 사용한다.
- Full PLAN에서 `validation_report`를 사용하는 경우 섹션명과 순서는 다음과 같이 유지한다.
  - `구조 검증 결과`
  - `입력 커버리지 검증 결과`
  - `변경 파일 상태`
  - `자동 수정 항목`
  - `잔여 이슈`
  - `환경 이슈`
  - `최종 상태`
- `변경 파일 상태`에는 tracked modified, deleted, untracked, staged 파일을 `Modified`, `Deleted`, `Untracked`, `Staged`로 구분해 기록한다.
- 변경 파일이 없는 분류도 생략하지 않고 해당 placeholder 값을 `없음`으로 채워 기록한다.
