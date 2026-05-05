<!-- Guide Tier: Mixed -->
<!-- Contains: P0 safety rules, P1 workflow rules, P2 reference rules -->
<!-- This file is an index for the writing/ sub-guides. -->

# Note Creation Guide

이 문서는 Markdown 강의노트 작성 규칙의 상위 인덱스다. 최종 노트를 작성할 때 먼저 이 문서로 핵심 원칙과 하위 파일 위치를 확인하고, 세부 작성 규칙은 `writing/` 하위 파일을 따른다.

## [P1] 1. 핵심 원칙

1. 최종 강의노트는 한국어가 기본이다.
2. 사용자가 명시적으로 요청한 경우에만 영어 노트를 작성한다.
3. 세부 내용, 예시, 도출 과정은 사용자가 요약을 요청하지 않는 한 생략하지 않는다.
4. 출력 언어, Markdown 서식, LaTeX 표기, 예시 형식의 구체적 작성 규칙은 전용 파일을 따른다.

## [P1] Output Language Policy
- 용도: 최종 마크다운 강의노트의 출력 언어 및 어투
- 파일: `agent_rules/writing/output_language_policy.md`

## [P0/P1] Markdown Note Style
- 용도: 파일명/저장 경로, 본문 생성 원칙, 문서 구조 및 서식
- 파일: `agent_rules/writing/markdown_note_style.md`

## [P1] LaTeX Style
- 용도: 수식 작성 규칙 (GitHub Flavored LaTeX)
- 파일: `agent_rules/writing/latex_style.md`

## [P2] Note Examples
- 용도: 마크다운 문서 내부의 예시 포맷
- 파일: `agent_rules/examples/note_examples.md`

## [P1] 2. 읽는 시점

- 출력 언어, 어투, 한국어 기본 원칙의 예외를 판단해야 하면 `agent_rules/writing/output_language_policy.md`를 읽는다.
- 파일명, 저장 경로, 문서 구조, 본문 서식 규칙을 적용해야 하면 `agent_rules/writing/markdown_note_style.md`를 읽는다.
- 수식 표기나 GitHub Flavored LaTeX 작성 방식이 필요하면 `agent_rules/writing/latex_style.md`를 읽는다.
- 문서 형식이 불명확하거나 사용자가 예시 확인을 요청하면 `agent_rules/examples/note_examples.md`를 읽는다.
