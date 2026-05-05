# Git and File Changes

## [P0/P1] 3. 작업 전 확인 절차

### [P1] 3.1 Workspace 확인
- 현재 디렉토리가 프로젝트 workspace/repository root인지 확인한다.
- 모든 상대 경로는 workspace root 기준으로 처리한다.
- 경로에 한글이 포함된 경우 반드시 따옴표로 감싼다.

### [P0] 3.2 Git 상태 확인
- `agent_rules/schemas/codex_policy.json`의 `git_checks.before_work`에 명시된 명령어 세트를 환경에 맞게 실행하여 현재 변경 상태를 파악한다.
- 작업 전 `git status --short`를 확인해 기존 modified/deleted/untracked 상태를 기록한다.
- 한글 경로가 octal escape 또는 따옴표 이스케이프 형태로 표시되면 작업 실패로 보지 않는다.
- Git 한글 경로 가독성이 필요한 경우 `git config core.quotepath false` 설정을 권장한다.

### [P0] 3.3 기존 변경 보호
- 사용자가 만든 기존 변경사항을 임의로 덮어쓰지 않는다.
- 내가 변경하지 않은 파일의 diff를 임의로 수정하지 않는다.
- 충돌이 의심되면 PLAN 파일에 기록하고 대기한다.

## [P0/P1] 4. 파일 변경 원칙
- 요청받은 과목/주차/차시에 해당하는 파일만 수정한다. (관련 없는 파일 수정 금지)
- 본문 작성이 끝난 뒤 반드시 PLAN 체크박스를 업데이트한다.
- 긴 문서는 H2 또는 H3 단위로 원자적 저장을 수행한다.
- manifest 자동 생성 및 상태 변경은 `task_type`이 `new_note` 또는 `update_note`로 확정된 실제 문서 작성/수정 작업에 한정한다.
- `review`, `dry_run`, `planning_only`, `summary`에서는 `inbox/`와 manifest를 read-only로 취급하고 필요한 변경은 제안만 보고한다.
- 단, `planning_only`에서는 사용자가 명시적으로 요청한 경우 PLAN 생성/갱신만 허용한다.

## [P0/P1] 8. 작업 후 확인 절차

### [P0] 8.1 변경 내역 확인
- `agent_rules/schemas/codex_policy.json`의 `git_checks.after_work`에 명시된 절차를 사용하여 방금 수행한 파일 변경 내역(diff)을 최종 확인한다.
- 작업 후 `git status --short`, `git diff --stat`, `git diff --cached --stat`을 함께 확인한다.
- `git status --short`를 기준으로 `Modified`, `Deleted`, `Untracked`, `Staged` 파일을 분리한다.
- tracked 수정 파일이 있으면 해당 파일 목록을 대상으로 `git diff -- <tracked modified files>`를 실행한다.
- 삭제 파일은 `git status --short`에서 별도로 기록하며, 필수 diff 대상으로 넘기지 않는다.
- untracked 파일과 폴더는 `git diff`에 나오지 않으므로 `git status --short`에서 별도 확인하고, 필요한 경우 파일 내용 또는 생성 목적을 검증한다.
- 신규 산출물은 untracked 상태여도 파일 목록, 생성 목적, 내용 검증 결과를 최종 보고서에 기록한다.
- 최종 보고서에는 modified, deleted, untracked, staged 변경을 구분해 적는다.
- 작업 전부터 존재하던 dirty 상태와 이번 작업으로 생긴 변경을 혼동하지 않도록, 가능한 경우 작업 전/후 상태 차이를 함께 기록한다.
