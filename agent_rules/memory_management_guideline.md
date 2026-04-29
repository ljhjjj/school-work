# Markdown Editorial Agent: Task & Memory Management Guidelines

## PLAN Language Policy
- PLAN files may mix English and Korean.
- Checkbox labels, phase names, task states, and technical workflow logs may be written in English.
- Lecture topic names, source interpretation notes, reading issues, unresolved issues, and user-facing confirmation notes may be written in Korean.
- The PLAN language does not determine the final lecture note language.
- Final markdown lecture notes must follow the output language policy in `note_creation_guide.md`.

## 1. 핵심 원칙 (Core Principles)
1. **일급 객체화 및 격리 원칙:** 단기 기억력 한계 극복 및 병렬 작업 충돌 방지를 위해, 계획 파일은 반드시 각 과목 폴더 내에 **차시 단위로 분리**하여 관리합니다.
2. **기본 연속 실행 원칙 (Continuous Execution by Default):** 특별한 블로커나 시스템 에러가 없는 한, 사용자의 개입 없이 계획된 목차의 작성을 끝까지 연속 실행합니다. 단, 불확실한 자료를 본문에 무리하게 병합하지 않습니다.

## 2. 작업 루틴 (Execution Loop)
새로운 문서 정리 요구사항을 받거나 작업을 재개할 때 다음 프로세스를 엄격히 따르세요:

1. **READ & INIT (읽기 및 초기화):** 
   - 셸 명령어(PowerShell: `Get-Content -Raw`, CMD: `type`, Bash: `cat`)로 **타겟 PLAN 파일(`[과목명]/[과목명]_[주차]-[차시]_PLAN.md`)**을 찾아 읽습니다. 파일이 없으면 새로 생성합니다.
2. **THINK:** `agent_rules/input_guide.md`를 참고하여, 파일 처리 규칙 및 우선순위 병합 지침을 실행합니다.
3. **ACT:** `_PLAN.md`의 목표를 확인한 뒤, `agent_rules/note_creation_guide.md`를 참고하여 본문 작성을 시작합니다.
4. **UPDATE (상태 업데이트 및 연속 진행):** 
   - 소주제(H3) 작성이 완료되면 즉시 `_PLAN.md`의 체크박스를 `[x]` 처리하고, 묻지 않고 다음 파트를 이어서 작성합니다.
   - 범위 대응이 불확실한 경우 사용자에게 즉시 질문하지 않습니다. 불확실한 내용은 본문에 포함하지 않고 PLAN의 이슈 또는 범위 기록에 `[대응 불확실]`, `[확인 필요]`, `[자료 PDF 초과 범위]` 등으로 남긴 뒤 다음 섹션을 진행합니다.
5. **TROUBLESHOOT (데이터 누락 및 시스템 예외 대기):** 
   - **[blocked 전환 조건]:** `blocked`는 데이터 누락, 판독 불가, 이후 전개 전체의 핵심 전제 불확실성으로 한정합니다.
   - **[데이터 이슈]:** 심각한 데이터 누락 또는 핵심 전제 판독 불가 시 PLAN 파일에 이슈를 기록하고 사용자에게 캡처본을 요청합니다. 단, 재요청은 **최대 2회**로 제한하며, PLAN 파일에 시도 횟수를 기록합니다(예: `- *Retry Count:* 1/2`). 실패 시 `> ⚠️ [수식/내용 판독 불가: 수동 입력 필요]` 태그를 남기고 넘어갑니다.
   - **[범위 불확실성]:** 범위 대응이 불확실하지만 이후 전개 전체의 핵심 전제가 아니면 `blocked`로 전환하지 않습니다. 본문에는 포함하지 않고 PLAN의 이슈 또는 범위 기록에 남긴 뒤 다음 섹션을 진행합니다.
   - **[시스템/실행 이슈]:** 토큰 초과, 툴 에러, 세션 종료 등으로 자율 연속 실행이 불가능해진 경우 무리하게 진행하지 마세요. 현재까지의 완료 범위를 체크하고 다음 재개 지점을 기록한 뒤 대기합니다.
6. **REVIEW & VALIDATE (선택적 웹 검증 및 사후 리포트):** 
   - **[제한적 검색 원칙]:** 전체 검색 금지. 오직 STT 오류가 의심되는 전문 용어, 보편적 공식 오타에 한해서만 웹 검증을 수행합니다.
   - **[고유 표기법 우선]:** 웹과 다르더라도 원본 수업 표기와 논리가 항상 최우선입니다.
   - 저장 완료 후 "[수정/추정 내역 요약] 및 [잔여/실행 이슈]"를 리포트로 출력합니다.

--- ( [과목명]_[주차]-[차시]_PLAN.md 내부 예시 포맷 ) ---

### 📋 목표: 고체역학 3주차 1차시 (응력과 변형률)
- **타겟 출력 파일:** `고체역학/고체역학 3-1.md`

#### Phase 1: 원본 데이터 분석 및 목차 설계
- [x] 1단계: 교차 검증 및 병합 기준 수립

#### Phase 2: 본문 마크다운 변환
- [x] 1. 정상 상태 정상 유동 (SSSF) 개요 작성
- [ ] 2. 노즐 및 디퓨저 (Nozzles and Diffusers) 👈 **(현재 포커스 / 재개 지점)**
- [ ] 3. 압축기 및 펌프 파트 
  - [x] *Blocker 처리 종료:* 2회 시도 실패, 판독 불가 태그 삽입
  - [ ] *본문 작성:* 판독 불가 태그를 포함하여 다음 파트 진행

#### ⚠️ 실행 이슈 (Execution Issues)
- **발생일시:** 2026-04-28
- **이슈:** 펌프 위치 에너지 수식 하단부 판독 불가. 사용자에게 선명한 캡처본 요청함.
- *Retry Count:* 1/2
- **조치:** H2 '정상 상태 유동' 파트까지 저장 완료. 다음 재개 시 '2. 노즐 및 디퓨저' 파트부터 이어서 작성 요망.

#### Phase 3: 최종 검수 및 선택적 웹 검증
- [ ] 1단계: 전문 용어 제한적 웹 검색 검증 및 자동 수정 (대기 없이 연속 실행)
- [ ] 2단계: 최종 리포트 출력
