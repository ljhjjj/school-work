# Report Validation

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
- 최종 리포트는 `agent_rules/schemas/report_templates.json`의 `template_selection`에 따라 선택한다.
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
