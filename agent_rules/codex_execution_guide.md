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

### 3.3 기존 변경 보호
- 사용자가 만든 기존 변경사항을 임의로 덮어쓰지 않는다.
- 내가 변경하지 않은 파일의 diff를 임의로 수정하지 않는다.
- 충돌이 의심되면 PLAN 파일에 기록하고 대기한다.

### 3.4 필수 설정 파일(JSON) 누락 시 예외 처리 (Fallback)
- `schemas/` 폴더 내에 필수 설정 파일(`schemas/validation_rules.json`, `schemas/codex_policy.json`, `schemas/report_templates.json`)이 존재하지 않거나 읽을 수 없는 경우 무한 대기하거나 작업을 중단하지 않는다.
- 파일 누락이 확인되면, 에이전트의 자체적인 기본 지식(보편적인 마크다운 H1/H2 구조, 일반적인 파일명 규칙 등)을 바탕으로 검증과 작업을 강행한다.
- 작업 완료 후 최종 리포트 및 PLAN의 `[⚠️ 실행 이슈]`에 **"필수 설정 파일(JSON) 누락으로 인해 기본 규칙으로 대체 실행됨"** 사실을 반드시 명시한다.

## 4. 파일 변경 원칙
- 요청받은 과목/주차/차시에 해당하는 파일만 수정한다. (관련 없는 파일 수정 금지)
- 본문 작성이 끝난 뒤 반드시 PLAN 체크박스를 업데이트한다.
- 긴 문서는 H2 또는 H3 단위로 원자적 저장을 수행한다.

## 5. 명령 실행 원칙

### 5.1 Windows 호환성, 인코딩 강제 및 권한 우회 (중요)
- **인코딩 강제 (CP949 깨짐 방지):** PowerShell 환경에서 한글 깨짐을 막기 위해 입출력 시 반드시 UTF-8 인코딩 파라미터를 강제한다.
  - 읽기 예: `Get-Content -Raw -Encoding UTF8 "파일경로"`
  - 쓰기/수정 예: `Out-File -Encoding UTF8 -FilePath "파일경로"` 또는 `Set-Content -Encoding UTF8 -Path "파일경로"`
  - PowerShell 7 이상에서는 가능하면 `-Encoding utf8NoBOM`을 우선 사용한다.
  - Windows PowerShell 5.1에서는 `-Encoding UTF8`을 사용한다.

- **실행 권한(Execution Policy) 블로킹 방지:** PowerShell 스크립트(`.ps1`) 실행이 시스템 정책에 의해 차단되는 것을 막기 위해, 가급적 파이프라인을 활용한 **단일 줄 명령어(One-liner)**로 명령을 구성한다.
  - 만약 스크립트 파일을 직접 실행해야 한다면 `-ExecutionPolicy Bypass` 플래그를 명시하여 권한을 우회한다.
  - `-ExecutionPolicy Bypass`는 repo 내부의 검증 스크립트에 한해 사용한다.
  - 외부에서 내려받은 스크립트에는 사용하지 않는다.
  - 스크립트 실행 예: `powershell -ExecutionPolicy Bypass -File "script.ps1"`

- 폴더 생성 예: `New-Item -ItemType Directory -Force -Path "과목명"`
- Bash/Git Bash 환경일 경우 호환 명령(`cat`, `mkdir -p` 등)을 허용한다.
### 5.2 위험 명령 제한
- `schemas/codex_policy.json`의 `dangerous_commands` 목록을 확인한다. (파일이 없다면 `rm -rf`, `npm install` 등의 파괴적/설치 명령을 기본적으로 차단한다.)
- 목록에 포함된 명령은 사용자 명시 요청 없이 절대 임의로 실행하지 않는다.

### 5.3 설치 금지 기본값
검증 도구가 없다고 해서 패키지를 임의 설치하지 않는다. (수동 정규식 검사로 대체)

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

## 8. 작업 후 확인 절차

### 8.1 변경 내역 확인
- `schemas/codex_policy.json`의 `git_checks.after_work`에 명시된 명령어를 사용하여 방금 수행한 파일 변경 내역(diff)을 최종 확인한다.

### 8.2 작업 요약 및 최종 보고 형식
- 최종 리포트 및 PR 본문 출력 시 가능한 경우 `schemas/report_templates.json`의 `execution_summary` 또는 `pr_summary` 템플릿을 우선 사용한다. (템플릿 누락 시 일반적인 리포트 양식으로 간결하게 자체 출력)
