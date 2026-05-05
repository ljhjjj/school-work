# Command Safety

## [P0/P1] 5. 명령 실행 원칙

### [P0] 5.1 Windows 호환성 및 인코딩 강제 (중요)
- **PowerShell profile 오류 분리:** Windows PowerShell 명령은 가능한 경우 `-NoProfile`을 사용해 사용자 profile 로딩을 생략한다.
  - 권장 예: `powershell -NoProfile -Command "Get-Content -Raw -Encoding UTF8 '파일경로'"`
  - 반복되는 `profile.ps1` 접근 권한 오류는 작업 실패가 아니라 실행 환경 이슈로 분리 기록한다.
  - profile 오류가 명령 본문 실행 결과를 막지 않았다면 검증/작업 실패로 판정하지 않는다.
- **인코딩 강제 (CP949 깨짐 방지):** PowerShell 환경에서 한글 깨짐을 막기 위해 입출력 시 반드시 UTF-8 인코딩 파라미터를 강제한다.
  - 읽기 예: `Get-Content -Raw -Encoding UTF8 "파일경로"`
  - 쓰기/수정 예: `Out-File -Encoding UTF8 -FilePath "파일경로"` 또는 `Set-Content -Encoding UTF8 -Path "파일경로"`
  - 출력 인코딩 권장 설정: `[Console]::OutputEncoding = [System.Text.Encoding]::UTF8`
  - 외부 명령 출력 인코딩 권장 설정: `$OutputEncoding = [System.Text.Encoding]::UTF8`
  - PowerShell 7 이상에서는 가능하면 `-Encoding utf8NoBOM`을 우선 사용한다.
  - Windows PowerShell 5.1에서는 `-Encoding UTF8`을 사용한다.
  - 한글이 깨져 보이면 먼저 `Get-Content -Raw -Encoding UTF8 "파일경로"`로 재확인하여 파일 자체 문제와 콘솔 출력 문제를 구분한다.
  - 파일 자체가 UTF-8로 정상 디코딩되면 인코딩 깨짐은 환경 이슈로 기록하고, 파일 내용을 임의로 재작성하지 않는다.
  - 파일 자체가 UTF-8로 정상 디코딩되지 않거나 실제 본문에 깨진 문자열이 있으면 UTF-8로 재저장하고 변경 내역에 기록한다.

- **PowerShell 스크립트 실행 기준:** `.ps1` 스크립트를 실행하기 전에는 반드시 내용을 읽고 목적, 변경 범위, 외부 호출 여부를 확인한다.
  - 일반 실행은 `pwsh -File "script.ps1"` 또는 `powershell -NoProfile -File "script.ps1"`처럼 ExecutionPolicy를 우회하지 않는 방식을 우선한다.
  - Python 모듈형 도구는 가능하면 `python -m ...` 또는 지정된 Python 환경 명령을 사용한다.
  - 출처가 불명확하거나 외부에서 받은 스크립트에는 `ExecutionPolicy Bypass`를 사용하지 않는다.
  - repo 낭비 스크립트라도 무조건 신뢰하지 않는다. 실행 전 내용과 영향 범위를 확인한다.
  - ExecutionPolicy 우회가 불가피한 경우에는 사용자 승인 또는 명확한 필요성 기록이 있어야 하며, 최종 보고서에 우회 사유와 실행 범위를 남긴다.

- 폴더 생성 예: `New-Item -ItemType Directory -Force -Path "과목명"`
- Bash/Git Bash 환경일 경우 호환 명령(`cat`, `mkdir -p` 등)을 허용한다.

### [P0] 5.2 위험 명령 제한

- 위험 명령 분류 및 정책은 `agent_rules/schemas/codex_policy.json`의 `dangerous_commands`를 따른다.
- 조회 전용 명령과 변경 명령의 구분은 해당 JSON의 `read_only_commands`와 `requires_explicit_user_approval`을 따른다.
- Markdown 본문에는 카테고리별 세부 패턴을 반복 서술하지 않는다.
- JSON 누락 시에도 삭제, 초기화, 권한 우회, 외부 코드 실행 등 명백한 고위험 명령은 사용자 승인 없이 실행하지 않는다.

### [P0] 5.3 설치 금지 기본값
검증 도구가 없다고 해서 패키지를 임의 설치하지 않는다. (수동 정규식 검사로 대체)

### [P1] 5.4 네트워크 접근 원칙
- 웹 검증이나 외부 자료 확인은 공식 검색 도구, 사용자가 명시적으로 허용한 네트워크 접근, 또는 현재 실행 환경에서 허용된 방식만 사용한다.
- 네트워크 접근을 위해 로컬 Codex 설정 파일을 임의로 수정하거나 접근 가능성을 전제하지 않는다.
- 네트워크 접근이 불가능하면 웹 검증을 생략하고, 검증하지 못한 항목을 최종 보고서의 `환경 이슈` 또는 `잔여 이슈`에 기록한다.
