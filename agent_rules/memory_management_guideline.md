# Markdown Editorial Agent: Task & Memory Management Guidelines

## PLAN Language Policy
- PLAN files may mix English and Korean.
- Checkbox labels, phase names, task states, and technical workflow logs may be written in English.
- Lecture topic names, source interpretation notes, reading issues, unresolved issues, and user-facing confirmation notes may be written in Korean.
- The PLAN language does not determine the final lecture note language.
- Final markdown lecture notes must follow the output language policy in `note_creation_guide.md`.

## 1. 핵심 원칙 (Core Principles)
1. **일급 객체화 및 격리 원칙:** 단기 기억력 한계 극복 및 병렬 작업 충돌 방지를 위해, 계획 파일은 반드시 `output/[과목명]/` 폴더 내에 **차시 단위로 분리**하여 관리합니다.
2. **기본 연속 실행 원칙 (Continuous Execution by Default):** 특별한 블로커나 시스템 에러가 없는 한, 사용자의 개입 없이 계획된 목차의 작성을 끝까지 연속 실행합니다. 단, 불확실한 자료를 본문에 무리하게 병합하지 않습니다.
3. **질문 단계 분리 원칙:** 작업 시작에 필요한 대상 폴더, 과목, 주차, 차시, 처리할 manifest 또는 입력 세트, 작업 유형이 불명확하면 사용자에게 질문합니다. 본문 범위 대응이나 입력 자료 간 매칭이 일부 불확실한 경우에는 질문하지 않고 PLAN 또는 최종 보고서의 이슈/불확실성 섹션에 기록한 뒤 진행합니다.
4. **Manifest 변경 제한 원칙:** manifest 자동 생성 및 상태 변경은 사용자가 강의노트 생성, 갱신, 정리, 변환 등 실제 문서 작성 작업 실행을 요청한 경우에만 수행합니다. 검토, 분석, 작업 계획, dry-run, 규칙 질문에서는 `inbox/`를 read-only로 취급하고 manifest 상태를 변경하지 않습니다.

## 2. 작업 루틴 (Execution Loop)
새로운 문서 정리 요구사항을 받거나 작업을 재개할 때 다음 프로세스를 엄격히 따르세요:

1. **READ & INIT (읽기 및 초기화):** 
   - 셸 명령어(PowerShell: `Get-Content -Raw`, CMD: `type`, Bash: `cat`)로 **타겟 PLAN 파일(`output/[과목명]/[과목명]_[주차]-[차시]_PLAN.md`)**을 찾아 읽습니다. 파일이 없으면 새로 생성합니다.
2. **THINK:** `agent_rules/input_guide.md`를 참고하여, 파일 처리 규칙 및 우선순위 병합 지침을 실행합니다.
3. **ACT:** `_PLAN.md`의 목표를 확인한 뒤, `agent_rules/note_creation_guide.md`를 참고하여 본문 작성을 시작합니다.
4. **UPDATE (상태 업데이트 및 연속 진행):** 
   - 소주제(H3) 작성이 완료되면 즉시 `_PLAN.md`의 체크박스를 `[x]` 처리하고, 묻지 않고 다음 파트를 이어서 작성합니다.
   - 범위 대응이 불확실한 경우 사용자에게 즉시 질문하지 않습니다. 불확실한 내용은 본문에 포함하지 않고 PLAN의 이슈 또는 범위 기록에 `[대응 불확실]`, `[확인 필요]`, `[자료 PDF 초과 범위]` 등으로 남긴 뒤 다음 섹션을 진행합니다.
5. **TROUBLESHOOT (데이터 누락 및 시스템 예외 대기):** 
   - **[blocked 전환 조건]:** `blocked`는 데이터 누락, 판독 불가, 이후 전개 전체의 핵심 전제 불확실성으로 한정합니다.
   - **[질문해야 하는 데이터 이슈]:** 작업 대상 폴더, 과목, 주차, 차시, 처리할 manifest 또는 입력 세트, 작업 유형이 불명확하면 PLAN 파일에 상태를 기록하고 사용자에게 질문합니다. 심각한 데이터 누락 또는 핵심 전제 판독 불가 시에도 PLAN 파일에 이슈를 기록하고 사용자에게 캡처본을 요청합니다. 단, 재요청은 **최대 2회**로 제한하며, PLAN 파일에 시도 횟수를 기록합니다(예: `- *Retry Count:* 1/2`). 실패 시 `> ⚠️ [수식/내용 판독 불가: 수동 입력 필요]` 태그를 남기고 넘어갑니다.
   - **[범위 불확실성]:** 범위 대응이 불확실하지만 이후 전개 전체의 핵심 전제가 아니면 `blocked`로 전환하지 않습니다. 본문에는 포함하지 않고 PLAN의 이슈 또는 범위 기록에 남긴 뒤 다음 섹션을 진행합니다.
   - **[입력 매칭 불확실성]:** STT, PDF, 필기 자료 간 매칭이 불완전하거나 강의 범위 안의 해석 근거가 약한 경우에는 즉시 질문하지 않습니다. PLAN 또는 최종 보고서의 이슈/불확실성 섹션에 근거와 제외 사유를 기록하고 가능한 다음 작업을 진행합니다.
   - **[시스템/실행 이슈]:** 토큰 초과, 툴 에러, 세션 종료 등으로 자율 연속 실행이 불가능해진 경우 무리하게 진행하지 마세요. 현재까지의 완료 범위를 체크하고 다음 재개 지점을 기록한 뒤 대기합니다.
6. **REVIEW & VALIDATE (선택적 웹 검증 및 사후 리포트):** 
   - **[제한적 검색 원칙]:** 전체 검색 금지. 오직 STT 오류가 의심되는 전문 용어, 보편적 공식 오타에 한해서만 웹 검증을 수행합니다.
   - **[고유 표기법 우선]:** 웹과 다르더라도 원본 수업 표기와 논리가 항상 최우선입니다.
   - 저장 완료 후 "[수정/추정 내역 요약] 및 [잔여/실행 이슈]"를 리포트로 출력합니다.
   - `잔여 이슈 없음`은 환경 경고, 우회 처리, 판독 불확실성, 내용 보완 필요가 모두 없을 때만 사용합니다.
   - 작업이 성공했더라도 PDF 렌더링 우회, 텍스트 추출 실패, PowerShell/Git/Python 환경 경고, 판독 보정 내역은 비치명적 잔여 이슈 또는 환경 이슈로 분리 기록합니다.

--- ( [과목명]_[주차]-[차시]_PLAN.md 내부 예시 포맷 ) ---

### 📋 목표: 고체역학 3주차 1차시 (응력과 변형률)
- **타겟 출력 파일:** `output/고체역학/고체역학 3-1.md`
- **타겟 PLAN 파일:** `output/고체역학/고체역학_3-1_PLAN.md`

#### Phase 1: 원본 데이터 분석 및 목차 설계
- [x] 1단계: 교차 검증 및 병합 기준 수립

#### 페이지 판독 기록
- `handwritten_note.pdf` p.1: 확인한 핵심 개념, 공식, 도식, 용어를 짧게 기록
- `handwritten_note.pdf` p.2: 확인한 핵심 개념, 공식, 도식, 용어를 짧게 기록
- `handwritten_note.pdf` p.3: 도식 논증 및 reversible/reversed process 관련 핵심 항목을 짧게 기록
- `lecture_slide.pdf` p.3: 강의 개요 또는 목차 후보 확인
- `lecture_slide.pdf` p.10~12: 용어·공식 보조 확인. 본문 확장용으로 사용하지 않음
- STT TXT: 강의 흐름과 손글씨 PDF의 p.1~p.3 범위가 충돌하지 않는지 확인

#### 입력 커버리지 검증
- [ ] `handwritten_note.pdf` p.1 핵심 항목 반영 또는 제외 사유 기록
- [ ] `handwritten_note.pdf` p.2 도식/논증 반영 또는 제외 사유 기록
- [ ] `handwritten_note.pdf` p.3 도식 논증 및 reversible/reversed process 반영 또는 제외 사유 기록
- [ ] `lecture_slide.pdf`는 손글씨 PDF와 별도 입력으로 보고 용어·공식 보조 확인 목적에 맞게 사용
- [ ] STT TXT가 있는 경우 강의 흐름과 본문 범위가 충돌하지 않음
- [ ] 구조 검증 결과와 입력 커버리지 검증 결과를 분리해 최종 리포트에 기록

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

#### 잔여 이슈
- 치명적 잔여 이슈: 없음.
- 비치명적 판독/우회 이슈: PDF 텍스트 직접 추출이 불충분하여 페이지 렌더링 판독으로 전환함.
- 환경 이슈: PowerShell profile 권한 경고, 콘솔 인코딩 경고, Git 한글 경로 표시 문제 등은 산출물 품질과 분리해 기록함.
- 내용 보완 이슈: 보완이 필요한 입력 항목이 있으면 페이지와 항목을 명시하고, 보완 완료 시 완료 근거를 함께 기록함.

#### Phase 3: 최종 검수 및 선택적 웹 검증
- [ ] 1단계: 전문 용어 제한적 웹 검색 검증 및 자동 수정 (대기 없이 연속 실행)
- [ ] 2단계: 최종 리포트 출력
