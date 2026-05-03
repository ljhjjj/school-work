# Codex Execution Guide

## 1. 목적
Codex가 Windows 로컬 workspace/repository에서 파일을 생성, 수정, 검증, 보고할 때 따라야 할 실행 절차를 정의한다.

## 2. 실행 시점
- 파일 생성 전 / 기존 파일 수정 전
- PLAN 파일 업데이트 전후 / 마크다운 본문 저장 후
- 검증 명령 실행 전후 / 최종 보고 전

## 3. 작업 전 확인 절차

### 3.1 Workspace 확인
- 현재 디렉토리가 프로젝트 workspace/repository root인지 확인한다.
- 모든 상대 경로는 workspace root 기준으로 처리한다.
- 경로에 한글이 포함된 경우 반드시 따옴표로 감싼다.

### 3.2 Git 상태 확인
- `schemas/codex_policy.json`의 `git_checks.before_work`에 명시된 명령어 세트를 환경에 맞게 실행하여 현재 변경 상태를 파악한다.
- 작업 전 `git status --short`를 확인해 기존 modified/deleted/untracked 상태를 기록한다.
- 한글 경로가 octal escape 또는 따옴표 이스케이프 형태로 표시되면 작업 실패로 보지 않는다.
- Git 한글 경로 가독성이 필요한 경우 `git config core.quotepath false` 설정을 권장한다.

### 3.3 기존 변경 보호
- 사용자가 만든 기존 변경사항을 임의로 덮어쓰지 않는다.
- 내가 변경하지 않은 파일의 diff를 임의로 수정하지 않는다.
- 충돌이 의심되면 PLAN 파일에 기록하고 대기한다.

### 3.4 필수 설정 파일(JSON) 누락 시 예외 처리 (Fallback)
- `schemas/` 폴더 내에 필수 설정 파일(`schemas/validation_rules.json`, `schemas/codex_policy.json`, `schemas/report_templates.json`)이 존재하지 않거나 읽을 수 없는 경우 무한 대기하거나 작업을 중단하지 않는다.
- 파일 누락이 확인되면, 에이전트의 자체적인 기본 지식(보편적인 마크다운 H1/H2 구조, 일반적인 파일명 규칙 등)을 바탕으로 검증과 작업을 강행한다.
- 작업 완료 후 최종 리포트 및 PLAN의 `[⚠️ 실행 이슈]`에 **"필수 설정 파일(JSON) 누락으로 인해 기본 규칙으로 대체 실행됨"** 사실을 반드시 명시한다.

### 3.5 가이드 재확인 기준
- 같은 목적의 가이드 문서 반복 읽기로 실행을 지연시키지 않는다.
- 작업 재개, 검증 실패 원인 분석, 파일 변경 후 컨텍스트 손실, 규칙 충돌 확인이 필요한 경우에는 관련 가이드 문서를 다시 확인할 수 있다.
- 가이드를 재확인한 뒤에는 확인한 규칙을 적용해 다음 작업으로 진행한다.

### 3.6 사용자 질문 기준
- 작업 시작에 필요한 대상 폴더, 과목, 주차, 차시, 처리할 manifest 또는 입력 세트, 작업 유형이 불명확한 경우에만 사용자에게 질문한다.
- 본문 범위 대응, STT/PDF/필기 자료 간 매칭, 강의 범위 안의 일부 해석 근거가 불확실한 경우에는 즉시 질문하지 않는다.
- 질문하지 않는 불확실성은 PLAN 또는 최종 보고서의 이슈/불확실성 섹션에 기록하고, 본문에는 단정적으로 병합하지 않는다.

## 4. 파일 변경 원칙
- 요청받은 과목/주차/차시에 해당하는 파일만 수정한다. (관련 없는 파일 수정 금지)
- 본문 작성이 끝난 뒤 반드시 PLAN 체크박스를 업데이트한다.
- 긴 문서는 H2 또는 H3 단위로 원자적 저장을 수행한다.
- manifest 자동 생성 및 상태 변경은 강의노트 생성, 갱신, 정리, 변환 등 실제 문서 작성 작업 실행 요청에 한정한다.
- 검토, 분석, 작업 계획, dry-run, 규칙 질문에서는 `inbox/`와 manifest를 read-only로 취급하고 필요한 변경은 제안만 보고한다.

## 5. 명령 실행 원칙

### 5.1 Windows 호환성 및 인코딩 강제 (중요)
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
  - repo 내부 스크립트라도 무조건 신뢰하지 않는다. 실행 전 내용과 영향 범위를 확인한다.
  - ExecutionPolicy 우회가 불가피한 경우에는 사용자 승인 또는 명확한 필요성 기록이 있어야 하며, 최종 보고서에 우회 사유와 실행 범위를 남긴다.

- 폴더 생성 예: `New-Item -ItemType Directory -Force -Path "과목명"`
- Bash/Git Bash 환경일 경우 호환 명령(`cat`, `mkdir -p` 등)을 허용한다.
### 5.2 위험 명령 제한
- `schemas/codex_policy.json`의 `dangerous_commands`를 확인한다. 위험 명령은 명령 이름이 아니라 실제 행위 기준으로 판단한다.
- 단순 조회 명령은 허용하되, 변경 작업과 결합되어 있으면 위험 명령으로 본다.
- 삭제/파괴, 대량 이동/덮어쓰기, git 히스토리 변경, 브랜치 전환/복원, 원격 전송, 외부 코드 다운로드 후 실행, 패키지 설치/환경 변경, 권한 상승/실행 정책 우회는 사용자 명시 요청 없이 실행하지 않는다.
- PowerShell 별칭(`rm`, `ri`, `del`, `erase`)과 다운로드 후 실행 패턴(`curl ... | powershell`, `Invoke-WebRequest ...; powershell ...`)도 같은 기준으로 판단한다.
- `git status`, `git diff`, `git branch --show-current`처럼 조회만 수행하는 명령은 브랜치 전환이나 파일 복원 명령과 구분한다.

### 5.3 설치 금지 기본값
검증 도구가 없다고 해서 패키지를 임의 설치하지 않는다. (수동 정규식 검사로 대체)

### 5.4 네트워크 접근 원칙
- 웹 검증이나 외부 자료 확인은 공식 검색 도구, 사용자가 명시적으로 허용한 네트워크 접근, 또는 현재 실행 환경에서 허용된 방식만 사용한다.
- 네트워크 접근을 위해 로컬 Codex 설정 파일을 임의로 수정하거나 접근 가능성을 전제하지 않는다.
- 네트워크 접근이 불가능하면 웹 검증을 생략하고, 검증하지 못한 항목을 최종 보고서의 `환경 이슈` 또는 `잔여 이슈`에 기록한다.

### 5.5 Python 실행 환경 고정
- PDF 처리, 페이지 렌더링, OCR, 검증 보조 스크립트 실행에는 기본적으로 `conda run -n pdfkit310 python`을 사용한다.
- 환경 점검 명령은 `conda run -n pdfkit310 python --version`을 우선 사용한다.
- 일반 텍스트 처리나 간단한 로컬 확인에 한해 repo 정책상 문제가 없으면 `python`을 사용할 수 있으나, PDF/OCR 작업의 기준 환경으로 보지 않는다.
- Windows `py` 런처는 WindowsApps 경로 문제로 실패할 수 있으므로 기본 검증 명령에서 제외한다.
- `py --version` 실패는 전체 작업 실패가 아니라 Python 런처 환경 이슈로 기록한다.
- `conda run -n pdfkit310 python`이 실패하면 `python --version`, `Get-Command python`, `conda env list` 결과를 확인하고, 대체 실행 경로를 PLAN 또는 최종 보고서의 `[환경 이슈]`에 기록한다.

### 5.6 PDF 페이지 렌더링 방식 우선순위
- PDF 판독용 이미지는 각 페이지를 독립 이미지로 렌더링하는 방식을 기본으로 한다.
- 렌더링 우선순위는 다음 순서를 따른다.
  1. Poppler 또는 MuPDF 기반 페이지별 렌더링
  2. Windows.Data.Pdf 기반 페이지별 렌더링
  3. Edge screenshot은 최후의 임시 확인용으로만 사용
- Edge headless screenshot은 저장 권한, viewport 캡처, 다중 페이지 처리 한계가 있으므로 기본 판독 경로로 사용하지 않는다.
- 렌더링 결과 파일명은 `{source_basename}_page_{page_number}.png` 형식을 기본으로 한다.
  - 예: `handwritten_note_page_001.png`, `lecture_slide_page_012.png`
- 렌더링 실패 시 권한 문제, 경로 문제, 도구 문제, 입력 PDF 문제를 구분해 PLAN 또는 최종 보고서의 `[환경 이슈]`나 `[잔여 이슈]`에 기록한다.
- 렌더링 도구 실패가 있어도 다른 페이지별 렌더링 방식으로 전환 가능하면 작업 실패로 판정하지 않는다.

### 5.7 렌더링 이미지 캐시 보관 모드
- 기본 모드에서는 PDF 판독용 렌더링 이미지를 작업 종료 후 삭제한다.
- 디버그 모드에서는 렌더링 이미지를 `output/_render_cache/{subject}/{lecture_or_unit}/` 아래에 보관한다.
  - 예: `output/_render_cache/열역학/8-1/handwritten_note_page_001.png`
- 새 PDF 판독 워크플로를 검증하거나 판독 근거 재검토 가능성이 크면 디버그 모드를 우선 사용한다.
- 렌더링 캐시를 보관할 때는 페이지별 이미지가 나중에 원본 PDF와 대응되도록 파일명을 `{source_basename}_page_{page_number}.png` 형식으로 유지한다.
- 작업 종료 시 PLAN 또는 최종 보고서에 렌더링 캐시 상태를 `삭제됨`, `보관됨`, `생성 실패` 중 하나로 기록한다.
- 캐시 삭제 실패는 산출물 실패가 아니라 정리 이슈로 기록하고, 보관된 경로를 함께 남긴다.

## 6. 검증 실행 절차
파일 변경 후 가능한 경우 다음 순서로 검증한다.
1. `validation_guide.md` 및 `schemas/validation_rules.json` 읽기 및 검사 수행 (없을 시 기본 마크다운 룰 적용)
2. PLAN 체크박스와 실제 파일 비교
3. 미해결 warning 태그 검색 
4. 검증 결과를 PLAN 및 최종 리포트에 반영

## 7. 실패 처리 정책

### 7.1 데이터 및 시스템 이슈
- 원본 누락, 판독 불가, 시스템 툴 에러 등은 이슈로 기록한다.
- 무리하게 진행하지 않고 현재까지 완료한 범위와 다음 재개 지점을 PLAN 파일에 남긴다.

### 7.2 환경 이슈와 작업 실패 분리
- PowerShell `profile.ps1` 권한 오류, 콘솔 인코딩 깨짐, Git 한글 경로 escape 표시처럼 산출물 내용과 직접 무관한 문제는 **환경 이슈**로 기록한다.
- 환경 이슈가 파일 수정, 검증 명령 실행, 산출물 저장을 실제로 막은 경우에만 작업 실패 또는 blocked 조건으로 승격한다.
- 최종 보고서에는 `작업 실패/검증 실패`와 `환경 이슈`를 분리해 적는다.
- 환경 이슈에는 가능한 경우 재현 명령과 권장 조치를 함께 남긴다.

## 8. 작업 후 확인 절차

### 8.1 변경 내역 확인
- `schemas/codex_policy.json`의 `git_checks.after_work`에 명시된 절차를 사용하여 방금 수행한 파일 변경 내역(diff)을 최종 확인한다.
- 작업 후 `git status --short`, `git diff --stat`, `git diff --cached --stat`을 함께 확인한다.
- `git status --short`를 기준으로 `Modified`, `Deleted`, `Untracked`, `Staged` 파일을 분리한다.
- tracked 수정 파일이 있으면 해당 파일 목록을 대상으로 `git diff -- <tracked modified files>`를 실행한다.
- 삭제 파일은 `git status --short`에서 별도로 기록하며, 필수 diff 대상으로 넘기지 않는다.
- untracked 파일과 폴더는 `git diff`에 나오지 않으므로 `git status --short`에서 별도 확인하고, 필요한 경우 파일 내용 또는 생성 목적을 검증한다.
- 신규 산출물은 untracked 상태여도 파일 목록, 생성 목적, 내용 검증 결과를 최종 보고서에 기록한다.
- 최종 보고서에는 modified, deleted, untracked, staged 변경을 구분해 적는다.
- 작업 전부터 존재하던 dirty 상태와 이번 작업으로 생긴 변경을 혼동하지 않도록, 가능한 경우 작업 전/후 상태 차이를 함께 기록한다.

### 8.2 작업 요약 및 최종 보고 형식
- 최종 리포트는 작업 유형에 맞는 `schemas/report_templates.json` 템플릿을 선택한다.
  - 강의노트 생성/갱신/정리/변환: `lecture_note_execution_summary`
  - 규칙 파일 검토/정리/일관성 수정: `rule_review_summary`
  - 단순 파일 수정: `simple_file_change_summary`
  - 검증만 수행: `validation_only_summary`
- `execution_summary`는 하위 호환용 별칭으로만 사용하고, PR 본문이 필요한 경우에만 `pr_summary`를 사용한다.
- 템플릿이 누락된 경우에는 작업 유형에 맞는 일반 리포트 양식으로 간결하게 자체 출력한다.
