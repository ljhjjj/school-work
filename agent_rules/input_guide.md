<!-- Guide Tier: Mixed -->
<!-- Contains: P0 safety rules, P1 workflow rules, P2 reference rules -->

# Input_Guide

## [P1] 1. 역할

이 문서는 입력 처리 규칙의 상위 인덱스다. 새로운 강의 노트 생성/수정 작업에서 입력 파일을 해석할 때 먼저 이 문서로 핵심 원칙과 하위 파일 위치를 확인하고, 세부 판정은 전용 파일을 따른다.

## [P1] 2. 핵심 원칙

1. 본문 범위는 파일 조합별 `primary_scope_anchor`가 결정한다.
2. 보조 자료와 외부 자료는 명시적으로 허용된 경우를 제외하고 본문 범위를 확장하지 않는다.
3. 판독 불확실성은 본문에 단정적으로 병합하지 않고 이슈, 추정 내역, 또는 PLAN 기록으로 남긴다.
4. PDF 판독 전략, 불확실성 태그 정의, 페이지 판독 기록 방식은 각 하위 파일 기준을 따른다.

## [P1] 3. 하위 파일 참조

- 파일 조합별 본문 범위와 `primary_scope_anchor` 적용 기준: `agent_rules/input/scope_anchor_rules.md`
- PDF 문서 유형 판별, 텍스트 추출 fallback, 페이지 렌더링/OCR 전환 기준: `agent_rules/input/pdf_reading_strategy.md`
- 정상 판정, 추정, 보류, 잔여 이슈 등 불확실성 태그 정의: `agent_rules/input/uncertainty_tags.md`
- 입력 판정, 판독 경로, PLAN 기록 방식: `agent_rules/input/input_plan_logging.md`
- 이미지 캡처본 기반 blocker 해결 절차: `agent_rules/input/image_troubleshooting.md`

## [P1] 4. 읽는 시점

- 어떤 입력 조합이 본문 범위를 결정하는지 판단해야 하면 `agent_rules/input/scope_anchor_rules.md`를 읽는다.
- PDF 유형 판별, 텍스트 추출 실패, 렌더링/OCR 전환이 필요하면 `agent_rules/input/pdf_reading_strategy.md`를 읽는다.
- 판독 결과를 확정, 추정, 보류 중 무엇으로 기록할지 애매하면 `agent_rules/input/uncertainty_tags.md`를 읽는다.
- 입력 판정 과정과 페이지 판독 내역을 PLAN에 남겨야 하면 `agent_rules/input/input_plan_logging.md`를 읽는다.
- `.jpg`, `.png` 캡처본으로 blocker를 해결해야 하면 `agent_rules/input/image_troubleshooting.md`를 읽는다.
