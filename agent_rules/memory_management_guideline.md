<!-- Guide Tier: Mixed -->
<!-- Contains: P0 safety rules, P1 workflow rules, P2 reference rules -->

# Markdown Editorial Agent: Task & Memory Management Guidelines

## [P1] PLAN Language Policy

PLAN 언어 기본값은 `agent_rules/plan/plan_language_policy.md`를 따른다.

## [P1] 1. 핵심 원칙 (Core Principles)
1. **일급 객체화 및 격리 원칙:** 단기 기억력 한계 극복 및 병렬 작업 충돌 방지를 위해, 계획 파일은 반드시 `output/[과목명]/` 폴더 내에 **차시 단위로 분리**하여 관리합니다.
2. **기본 연속 실행 원칙 (Continuous Execution by Default):** 특별한 블로커나 시스템 에러가 없는 한, 사용자의 개입 없이 계획된 목차의 작성을 끝까지 연속 실행합니다. 단, 불확실한 자료를 본문에 무리하게 병합하지 않습니다.
3. **질문 단계 분리 원칙:** 작업 시작에 필요한 대상 폴더, 과목, 주차, 차시, 처리할 manifest 또는 입력 세트, 작업 유형이 불명확하면 사용자에게 질문합니다. 본문 범위 대응이나 입력 자료 간 매칭이 일부 불확실한 경우에는 질문하지 않고 PLAN 또는 최종 보고서의 이슈/불확실성 섹션에 기록한 뒤 진행합니다.
4. **[P0] Manifest 변경 제한 원칙:** manifest 자동 생성 및 상태 변경은 사용자가 강의노트 생성, 갱신, 정리, 변환 등 실제 문서 작성 작업 실행을 요청한 경우에만 수행합니다. 검토, 분석, 작업 계획, dry-run, 규칙 질문에서는 `inbox/`를 read-only로 취급하고 manifest 상태를 변경하지 않습니다.

## [P1] 2. PLAN 템플릿 종류

Mini PLAN과 Full PLAN의 적용 기준 및 필수 섹션은 `agent_rules/plan/mini_full_plan.md`를 따른다.

## [P1] 3. 작업 루틴 (Execution Loop)
새로운 문서 정리 요구사항을 받거나 작업을 재개할 때 다음 프로세스를 엄격히 따르세요:

1. **READ & INIT (읽기 및 초기화):** 
   - 셸 명령어(PowerShell: `Get-Content -Raw`, CMD: `type`, Bash: `cat`)로 **타겟 PLAN 파일(`output/[과목명]/[과목명]_[주차]-[차시]_PLAN.md`)**을 찾아 읽습니다. 파일이 없으면 새로 생성합니다.
2. **THINK:** `agent_rules/input_guide.md`를 참고하여, 파일 처리 규칙 및 우선순위 병합 지침을 실행합니다.
3. **ACT:** `_PLAN.md`의 목표를 확인한 뒤, `agent_rules/note_creation_guide.md`를 참고하여 본문 작성을 시작합니다.
4. **UPDATE (상태 업데이트 및 연속 진행):** 
   - Mini PLAN: H2 섹션 완료 시에만 체크박스를 업데이트한다. H3 단위는 기록하지 않는다.
   - Full PLAN: H3 단위까지 체크박스를 업데이트할 수 있으나, 연속 실행이 불가능해지는 경우에만 상세히 기록한다.
   - 범위 대응이 불확실한 경우 사용자에게 즉시 질문하지 않는다. `[대응 불확실]` 등으로 기록하고 다음 섹션을 진행한다.
5. **TROUBLESHOOT (데이터 누락 및 시스템 예외 대기):**
   - blocked 전환, 재요청 제한, 재개 지점 기록은 `agent_rules/plan/blocker_and_resume.md`를 따른다.

6. **REVIEW & VALIDATE (선택적 웹 검증 및 사후 리포트):** 
   - **[제한적 검색 원칙]:** 전체 검색 금지. 오직 STT 오류가 의심되는 전문 용어, 보편적 공식 오타에 한해서만 웹 검증을 수행합니다.
   - **[고유 표기법 우선]:** 웹과 다르더라도 원본 수업 표기와 논리가 항상 최우선입니다.
   - 저장 완료 후 "[수정/추정 내역 요약] 및 [잔여/실행 이슈]"를 리포트로 출력합니다.
   - `잔여 이슈 없음`은 환경 경고, 우회 처리, 판독 불확실성, 내용 보완 필요가 모두 없을 때만 사용합니다.
   - 작업이 성공했더라도 PDF 렌더링 우회, 텍스트 추출 실패, PowerShell/Git/Python 환경 경고, 판독 보정 내역은 비치명적 잔여 이슈 또는 환경 이슈로 분리 기록합니다.

## [P2] PLAN 예시

상세 PLAN 예시는 `agent_rules/examples/plan_examples.md`를 참조한다.

예시는 형식이 불명확하거나 사용자가 예시 확인을 요청한 경우에만 읽는다.
