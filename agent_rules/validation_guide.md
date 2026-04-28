# Validation Guide

## 1. 목적
작성 완료된 마크다운 노트와 PLAN 파일이 프로젝트 규칙을 준수하는지 검증한다.
이 가이드는 본문 작성 후, 최종 리포트 출력 전에 반드시 실행되는 품질 게이트이다.

## 2. 실행 시점
- `[과목명] [주차]-[차시].md` 파일 저장 직후
- `_PLAN.md` 업데이트 직후
- 최종 리포트 출력 직전
- Codex 작업에서 파일 변경이 발생한 경우, 작업 종료 전

## 3. 필수 입력
- 타겟 마크다운 파일: `[과목명]/[과목명] [주차]-[차시].md`
- 타겟 PLAN 파일: `[과목명]/[과목명]_[주차]-[차시]_PLAN.md`
- 필수 설정 파일 (JSON):
  - `agent_rules/validation_rules.json` (경로, 구조, 수식, 경고 태그 검증 규칙)
  - `agent_rules/codex_policy.json` (자동 수정 정책)
  - `agent_rules/report_templates.json` (리포트 출력 양식)

## 4. Required Checks (validation_rules.json 참조)

### 4.1 파일 경로 및 이름 검사
- `validation_rules.json`의 `paths`와 `encoding` 규칙을 검사한다.
- 두 파일이 같은 과목, 같은 주차, 같은 차시를 가리키는지 확인한다.

### 4.2 Markdown 구조 검사
- `validation_rules.json`의 `markdown_structure` 정규식을 활용해 H1, H2, H3 구조를 검사한다.

### 4.3 수식 렌더링 검사
- `validation_rules.json`의 `latex` 규칙을 기반으로 독립 블록 여부, 금지 패턴, 경고 매크로 등을 검사한다.

### 4.4 콘텐츠 포맷 검사
- 개념 정의와 특징은 불릿 포인트 중심으로 정리되었는지 확인한다.
- 수식 도출과 논리 전개는 불릿으로 과도하게 압축되지 않았는지 확인한다.
- 예시와 부연 설명은 `> **예시:**` 블록으로 분리되었는지 확인한다.
- 문체가 `~한다`, `~이다` 형태의 객관적 문어체인지 확인한다.

### 4.5 PLAN 파일 일치 검사
- PLAN 파일의 타겟 출력 파일 경로와 실제 생성 파일 경로가 일치하는지 확인한다.
- 완료/미완료 항목의 `[x]`, `[ ]` 체크 상태와 재개 지점을 점검한다.

### 4.6 미해결 경고 태그 검사
- `validation_rules.json`의 `warning_tags.search_terms` 목록을 문서 및 PLAN에서 검색한다.
- `false_positive_exceptions`에 해당하는 섹션/문구는 오탐으로 간주하여 무시한다.
- 발견된 항목은 최종 리포트의 `[잔여 이슈]` 섹션에 반드시 포함한다.

### 4.7 선택적 도구 검사
- 도구(markdownlint 등)가 미설치된 경우 설치를 강행하지 말고, 스크립트/정규식 수동 검사로 대체한다.

## 5. Failure Policy (codex_policy.json 참조)

### 5.1 자동 수정 및 제한
- `codex_policy.json`의 `auto_fix_policy`를 기반으로 사용자 확인 없이 자동 수정을 진행한다.
- `requires_confirmation`에 해당하는 동작(파일 이동/삭제 등)은 사용자 확인을 대기한다.

### 5.2 수정 실패 시 처리
- 최대 시도 횟수(`max_attempts_per_error`)를 초과하면 PLAN의 검증 이슈 섹션에 기록하고 본문에 경고 태그를 남긴 뒤 다음 작업으로 진행한다.

## 6. Validation Report Format
- 최종 리포트 출력 시 가능한 경우 `agent_rules/report_templates.json`의 `validation_report` 템플릿을 우선 사용한다.