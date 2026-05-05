<!-- Guide Tier: Mixed -->
<!-- Contains: P0 safety rules, P1 workflow rules, P2 reference rules -->

# Codex Execution Guide

이 문서는 Codex 실행 가이드의 상위 인덱스다. 파일 변경 전후, 검증 명령 실행 전후, 최종 보고 전에 이 문서로 핵심 원칙과 하위 파일 위치를 확인하고, 상세 실행 절차는 `agent_rules/execution/` 하위 파일과 schema를 따른다.

## [P1] 1. 핵심 원칙

- 파일 변경 전후에는 execution 하위 가이드를 따른다.
- 위험 명령 제한, 기존 변경 보호, UTF-8 인코딩, git 상태 확인은 P0 안전 정책이다.
- 위험 명령과 git check의 기계적 기준은 `agent_rules/schemas/codex_policy.json`을 따른다.
- 최종 보고 템플릿은 `agent_rules/schemas/report_templates.json`을 따른다.
- JSON과 Markdown이 충돌할 경우 schema JSON을 우선한다.

## [P1] 2. P0 안전 정책 요약

- 위험 명령 제한: `agent_rules/schemas/codex_policy.json`의 `dangerous_commands`를 따른다.
- 기존 변경 보호: 사용자가 만든 기존 변경사항을 임의로 덮어쓰지 않는다.
- UTF-8 인코딩: PowerShell 환경에서 입출력 시 UTF-8 인코딩 파라미터를 명시한다.
- git 상태 확인: 작업 전후 `git status --short`를 확인하여 기존 변경과 이번 작업 변경을 구분한다.

## [P1] 3. 하위 파일 참조

- workspace 확인, git 상태, 기존 변경 보호, 파일 변경 원칙: `agent_rules/execution/git_and_file_changes.md`
- Windows 호환성, UTF-8 인코딩, PowerShell 실행, 위험 명령 제한: `agent_rules/execution/command_safety.md`
- Python 실행 환경, PDF 렌더링, 렌더링 이미지 캐시: `agent_rules/execution/python_and_pdf_rendering.md`
- JSON canonical source, 검증 실행, 실패 처리, 최종 보고: `agent_rules/execution/json_and_reporting.md`
- 위험 명령과 git check의 schema 기준: `agent_rules/schemas/codex_policy.json`
- 최종 보고 템플릿 schema: `agent_rules/schemas/report_templates.json`

## [P1] 4. 읽는 시점

- 작업 시작 전 workspace 상태와 기존 변경 보호 원칙을 확인해야 하면 `agent_rules/execution/git_and_file_changes.md`를 읽는다.
- PowerShell 실행, UTF-8 인코딩, 위험 명령 여부를 판단해야 하면 `agent_rules/execution/command_safety.md`를 읽는다.
- Python 실행 방식이나 PDF 렌더링 절차가 필요하면 `agent_rules/execution/python_and_pdf_rendering.md`를 읽는다.
- JSON 누락 처리, 검증 실행, 최종 보고 기준을 확인해야 하면 `agent_rules/execution/json_and_reporting.md`를 읽는다.
- 위험 명령 분류나 git check의 기계적 기준이 필요하면 `agent_rules/schemas/codex_policy.json`을 우선 확인한다.
- 최종 보고 템플릿과 상태 형식이 필요하면 `agent_rules/schemas/report_templates.json`을 우선 확인한다.
