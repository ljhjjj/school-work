# 열역학 8-2 PLAN

## 작업 범위
- 과목: 열역학
- 차시: 8-2
- 입력 폴더: `inbox/열역학/8-2`
- 주 범위 기준(primary_scope_anchor): `lecture_stt_20260503_153054.txt`
- 보조 자료: `lecture_slide(1).pdf`, `lecture_slide(2).pdf`
- 타겟 출력 파일: `output/열역학/열역학 8-2.md`
- 타겟 PLAN 파일: `output/열역학/열역학_8-2_PLAN.md`

## 입력 판정
- [x] `lecture_stt_20260503_153054.txt`는 강의 흐름과 세부 설명이 충분하여 본문 범위 기준(`primary_scope_anchor`)으로 사용함.
- [x] `lecture_slide(1).pdf`는 텍스트 레이어를 `pdftotext -layout -enc UTF-8`로 추출하여 Carnot corollaries, thermodynamic temperature scale, ideal gas temperature scale의 용어·공식 확인에 사용함.
- [x] `lecture_slide(2).pdf`는 텍스트 레이어를 `pdftotext -layout -enc UTF-8`로 추출하여 Carnot strips, Clausius inequality, 예제 수치 확인에 사용함.
- [x] `lecture_slide(2).pdf`의 Entropy 이후 슬라이드는 STT에서 "다음 시간"으로 미룬 범위이므로 본문 확장에 사용하지 않음.
- [x] 판독 경로: `lecture_stt_20260503_153054.txt` = `텍스트 활용`, `lecture_slide(1).pdf` = `텍스트 활용`, `lecture_slide(2).pdf` = `텍스트 활용`.

## Phase 1: 입력 판독 및 범위 확정
- [x] STT에서 이번 차시 핵심 범위를 확인함: 이상기체 온도척도, Clausius 부등식, 판별 예제.
- [x] 슬라이드 (1)에서 이상기체 Carnot 사이클 유도식과 부피비 관계를 확인함.
- [x] 슬라이드 (2)에서 Clausius 부등식 증명 구조와 예제 1~3을 확인함.
- [x] STT와 슬라이드 범위 충돌 여부를 확인함.

## Phase 2: 본문 작성
- [x] 지난 차시 복습과 이번 차시 목표를 정리함.
- [x] 이상기체 Carnot 사이클의 제1법칙 적용식을 정리함.
- [x] 등온 팽창/압축의 열전달식을 정리함.
- [x] 단열 팽창/압축에서 부피비 관계를 유도함.
- [x] 이상기체 온도척도와 열역학적 절대온도척도 일치를 정리함.
- [x] Carnot strips와 Clausius 부등식의 의미를 정리함.
- [x] 보조 가역기관을 이용한 Clausius 부등식 증명 구조를 정리함.
- [x] 전도, 비가역 열기관, Carnot 열기관 예제를 정리함.
- [x] 엔트로피는 본격 전개하지 않고 다음 차시 연결로 제한함.

## Phase 3: 검증
- [x] H1 1개와 번호형 H2 구조를 확인함.
- [x] 각 H2 끝에 구분선 `---`이 있는지 확인함.
- [x] 블록 수식이 독립 줄에 배치되었는지 확인함.
- [x] 입력 커버리지 검증을 수행함.
- [x] 작업 전후 git 상태를 확인함.

## 페이지 판독 기록
- `lecture_slide(1).pdf` p.1~3: 강의 제목, 일정, outline 확인. Carnot corollaries, thermodynamic temperature scale, ideal gas temperature scale이 슬라이드 범위임을 확인.
- `lecture_slide(1).pdf` p.4~16: Carnot corollaries와 thermodynamic temperature scale 전개 확인. STT에서는 복습으로만 다루어 본문에서는 배경 설명으로 제한.
- `lecture_slide(1).pdf` p.17~28: 열역학적 절대온도척도, Carnot 효율, refrigerator/heat pump COP, 예제 확인. STT에서 상세 풀이하지 않은 예제는 본문 확장에서 제외.
- `lecture_slide(1).pdf` p.29~37: ideal gas temperature scale과 Carnot cycle 유도식 확인. STT의 이상기체 온도척도 설명과 직접 대응되어 본문 반영.
- `lecture_slide(2).pdf` p.1~6: Entropy outline, Carnot strips, Clausius theorem/inequality 정의 확인. STT의 Carnot strips 및 Clausius 부등식 설명과 대응되어 본문 반영.
- `lecture_slide(2).pdf` p.7~22: Clausius inequality의 일반 증명 구조 확인. 보조 가역기관 A/B, $T_0$ 열저장조, $Q_i/T_i$ 합산 관계를 본문 반영.
- `lecture_slide(2).pdf` p.23~26: 전도, 비가역 열기관, Carnot 열기관 예제 확인. STT에서 설명한 판별식 예제로 본문 반영.
- `lecture_slide(2).pdf` p.27~44: Entropy, T-S diagram, Gibbs equations. STT에서 엔트로피는 다음 시간으로 미루었으므로 본문 확장 제외, 마지막 연결 설명에만 제한.

## 입력 커버리지 검증
- [x] STT의 "이상기체 온도척도는 열역학적 절대온도척도와 같아야 한다"는 설명을 본문 `## 5`에 반영함.
- [x] STT의 $du = C_v dT$는 변화량으로 써야 한다는 강조를 본문 `## 2`에 반영함.
- [x] STT의 등온 열추가/열방출 부호와 절댓값 설명을 본문 `## 3`에 반영함.
- [x] STT의 단열 팽창/압축 적분 결합과 부피비 관계를 본문 `## 4`에 반영함.
- [x] STT의 Carnot strips 설명을 본문 `## 6`에 반영함.
- [x] STT의 Clausius 부등식 판별식 해석을 본문 `## 6`과 `## 8`에 반영함.
- [x] STT의 보조 가역기관을 이용한 일반 증명 흐름을 본문 `## 7`에 반영함.
- [x] STT의 세 수치 예제와 불가능한 효율 사례를 본문 `## 8`에 반영함.
- [x] STT에서 다음 시간으로 미룬 엔트로피는 본문 `## 9`의 연결 설명으로만 제한함.

## 범위 기록
- [TXT 우선] 본문 범위는 `lecture_stt_20260503_153054.txt`의 실제 강의 흐름을 기준으로 함.
- [보정] 슬라이드 PDF는 STT의 용어, 수식, 예제 수치 확인용으로 사용함.
- [자료 PDF 초과 범위] `lecture_slide(1).pdf`의 Carnot corollary 상세 증명과 온도척도 예제 일부는 STT에서 이번 차시 본문으로 상세 설명되지 않아 배경 설명으로만 제한함.
- [자료 PDF 초과 범위] `lecture_slide(2).pdf`의 Entropy, T-S diagram, Gibbs equations는 STT에서 다음 시간으로 넘긴 범위이므로 본문에 전개하지 않음.

## 검증 로그
- `git status --short`
- `git diff --stat`
- `pdftotext -layout -enc UTF-8 "inbox/열역학/8-2/lecture_slide(1).pdf" -`
- `pdftotext -layout -enc UTF-8 "inbox/열역학/8-2/lecture_slide(2).pdf" -`
- 수동 검증: H1 1개, 번호형 H2, 번호형 H3, 독립 블록 수식, H2 끝 구분선, 입력 커버리지 확인

## 실행 이슈
- PDF Python 라이브러리(`fitz`, `pypdf`, `PyPDF2`)와 `pdftotext` PATH 등록은 없었으나, `C:\Program Files\Git\mingw64\bin\pdftotext.exe`를 직접 호출하여 텍스트 레이어를 판독함.

## 잔여 이슈
- 치명적 잔여 이슈: 없음.
- 비치명적 범위 제한: 슬라이드의 엔트로피 이후 내용은 STT상 다음 차시 범위로 판단하여 제외함.
