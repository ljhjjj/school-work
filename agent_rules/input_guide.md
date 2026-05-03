<!-- Guide Tier: Mixed -->
<!-- Contains: P0 safety rules, P1 workflow rules, P2 reference rules -->

# Input_Guide

## [P2] 1. 파일 유형 및 분류
- `.txt`: 강의 영상 자막 추출 데이터(STT)
- `.pdf`: PPT 또는 노트 필기 스캔본
- `.jpg` / `.png`: 트러블슈팅 캡처본

## [P1] 2. AI 행동 지침 (Data Processing & Workflow)
**[작업 범위 제한]: 새로운 강의 노트 생성/수정 작업에서 파일을 입력받거나 작업을 시작할 때** 다음 지침에 따라 처리하세요. (단순 질문 시 무시)

1. **정보 읽기 및 우선순위 병합 (Preprocessing)**
   - **[판독 신뢰도 기준]** PDF/TXT 간 표기 충돌을 판독할 때만 일반적으로 `노트 필기 PDF > PPT 형태 PDF > 자막 추출 TXT` 순으로 표기 정확도를 높게 본다.
   - 단, 이는 본문 범위 결정 우선순위가 아니다. 본문에 어떤 내용을 포함할지는 파일 조합별 `primary_scope_anchor`가 결정한다.
   - **[.txt 처리: 신뢰도 기반 3단계 STT 교정 원칙]:**
     - **[Tier 1] 확정:** PDF, STT, 문맥이 모두 일치하거나 표준 용어로서 대체 후보가 사실상 하나뿐인 경우. 👉 *본문 즉시 반영.*
     - **[Tier 2] 추정:** 문맥상 가능성이 높으나, 대체 가능한 유사 후보가 2개 이상인 경우. 👉 *가장 적합한 단어로 작성하되 최종 리포트에 추정 내역을 기록.*
     - **[Tier 3] 보류:** 판독 불가. 👉 *원문 유지 후 `[?]` 표시하고 이슈로 기록.*
   - PDF 문서 유형 판별, 텍스트 추출 fallback, 페이지 렌더링/OCR 전환 기준은 `agent_rules/input/pdf_reading_strategy.md`를 따른다.

### [P0] 파일 조합별 본문 범위 및 검증 우선순위(primary_scope_anchor)

파일 조합별 본문 범위와 `primary_scope_anchor` 적용 기준은 `agent_rules/input/scope_anchor_rules.md`를 따른다.

2. **DOC_PLAN 설계 및 작성 (Planning)**
   - 본문 작성 전, **해당 차시의 계획 파일(`output/[과목명]/[과목명]_[주차]-[차시]_PLAN.md`)**에 단계별 작업 계획을 먼저 작성하세요.
   - 입력 판정, 판독 경로, 페이지 판독 기록 방식은 `agent_rules/input/input_plan_logging.md`를 따른다.

3. **이미지 파일(`.jpg`, `.png`) 트러블슈팅 처리 규칙 (Resolving Blockers)**
   - 이미지 캡처본 기반 blocker 해결 규칙은 `agent_rules/input/image_troubleshooting.md`를 따른다.
