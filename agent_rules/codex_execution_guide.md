<!-- Guide Tier: Mixed -->
<!-- Contains: P0 safety rules, P1 workflow rules, P2 reference rules -->

# Codex Execution Guide

> 이 파일은 `agent_rules/execution/` 하위 파일들의 **인덱스**입니다.  
> 상세 실행 절차는 아래 개별 파일을 참조하세요.

---

## [P2] 1. 목적

Codex가 Windows 로컬 workspace/repository에서 파일을 생성, 수정, 검증, 보고할 때 따라야 할 실행 절차를 정의합니다.

---

## [P1] 2. 실행 시점

- 파일 생성 전 / 기존 파일 수정 전
- PLAN 파일 업데이트 전후 / 마크다운 본문 저장 후
- 검증 명령 실행 전후 / 최종 보고 전

---

## P0 안전 정책 요약

다음 정책은 파일 분리 후에도 그대로 유지됩니다.

- **위험 명령 제한**: `agent_rules/schemas/codex_policy.json`의 `dangerous_commands`를 따른다.
- **기존 변경 보호**: 사용자가 만든 기존 변경사항을 임의로 덮어쓰지 않는다.
- **UTF-8 인코딩**: PowerShell 환경에서 입출력 시 반드시 UTF-8 인코딩 파라미터를 강제한다.
- **Git 상태 확인**: 작업 전후 `git status --short`를 확인하여 기존 변경과 이번 작업 변경을 구분한다.

---

## 개별 가이드 파일

| 시점 / 주제 | 파일 |
|---|---|
| Workspace 확인, Git 상태, 기존 변경 보호, 파일 변경 원칙, 작업 후 변경 내역 확인 | `agent_rules/execution/git_and_file_changes.md` |
| Windows 호환성, UTF-8 인코딩, PowerShell 실행, 위험 명령 제한, 설치 금지, 네트워크 접근 | `agent_rules/execution/command_safety.md` |
| Python 실행 환경, PDF 렌더링, 렌더링 이미지 캐시 | `agent_rules/execution/python_and_pdf_rendering.md` |
| JSON 누락 처리, JSON Canonical Source, 가이드 재확인, 검증 실행, 실패 처리, 작업 요약 및 최종 보고 | `agent_rules/execution/json_and_reporting.md` |

---

## Canonical Source

- `agent_rules/schemas/codex_policy.json`은 위험 명령 분류 및 git 절차의 기계 해석 가능한 유일한 규칙(canonical source)입니다.
- `agent_rules/schemas/report_templates.json`은 최종 보고 템플릿 선택의 canonical source입니다.
- JSON과 Markdown이 충돌할 경우 JSON을 우선합니다.
- 단, 위험 명령 제한, 기존 변경 보호, UTF-8 인코딩 등 P0 안전 정책은 JSON 누락 여부와 무관하게 항상 유지합니다.
