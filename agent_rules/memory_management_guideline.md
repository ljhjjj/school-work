<!-- Guide Tier: Mixed -->
<!-- Contains: P0 safety rules, P1 workflow rules, P2 reference rules -->

# Markdown Editorial Agent: Task & Memory Management Guidelines

## [P1] 1. 역할

이 문서는 PLAN 관리 규칙의 상위 인덱스다. 실제 작성 작업에서 PLAN을 만들거나 갱신할 때 먼저 이 문서로 핵심 원칙과 하위 파일 위치를 확인하고, 세부 판단은 전용 파일을 따른다.

## [P1] 2. 핵심 원칙

1. PLAN은 `output/[과목명]/` 폴더 안에서 차시 단위로 분리해 관리한다.
2. PLAN 생성과 갱신은 사용자가 강의노트 생성, 갱신, 정리, 변환 등 실제 문서 작성 작업을 요청한 경우에만 수행한다.
3. 본문 범위 대응이나 입력 자료 간 매칭이 일부 불확실하면 본문에 무리하게 병합하지 않고 PLAN 또는 최종 보고서의 이슈/불확실성 섹션에 기록한다.
4. 작업 시작에 필요한 대상 폴더, 과목, 주차, 차시, 처리할 manifest 또는 입력 세트, 작업 유형이 불명확하면 사용자에게 질문한다.

## [P1] 3. 하위 파일 참조

- PLAN 언어 기본값과 예외 기준: `agent_rules/plan/plan_language_policy.md`
- Mini PLAN과 Full PLAN의 선택 기준, 필수 섹션, 업데이트 단위: `agent_rules/plan/mini_full_plan.md`
- blocked 전환, 재요청 제한, 재개 지점 기록: `agent_rules/plan/blocker_and_resume.md`
- PLAN 작성 예시와 형식 참고: `agent_rules/examples/plan_examples.md`

## [P1] 4. 읽는 시점

- PLAN 언어가 불명확하거나 사용자 언어와 출력 언어가 충돌하면 `agent_rules/plan/plan_language_policy.md`를 읽는다.
- 작업 규모에 맞는 PLAN 형식을 골라야 하거나 체크박스 갱신 단위를 판단해야 하면 `agent_rules/plan/mini_full_plan.md`를 읽는다.
- 자료 누락, 시스템 예외, 재요청, 재개 처리가 필요하면 `agent_rules/plan/blocker_and_resume.md`를 읽는다.
- 사용자가 예시 확인을 요청했거나 PLAN 형식이 불명확하면 `agent_rules/examples/plan_examples.md`를 읽는다.
