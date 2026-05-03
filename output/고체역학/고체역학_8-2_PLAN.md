# 고체역학 8-2 PLAN

## 작업 범위
- 과목: 고체역학
- 차시: 8-2
- 입력 폴더: `inbox/고체역학/8-2`
- 대상 출력 파일: `output/고체역학/고체역학 8-2.md`
- 대상 PLAN 파일: `output/고체역학/고체역학_8-2_PLAN.md`
- 주 범위 기준(primary_scope_anchor): `lecture_slide.pdf`

## 입력 판정
- [x] 기존 `input_manifest.json`을 실제 입력 기준으로 처음부터 재생성함.
- [x] `lecture_slide.pdf`만 제공되어 해당 PDF 전체를 본문 범위 기준(`primary_scope_anchor`)으로 사용함.
- [x] `lecture_slide.pdf`는 28쪽 PDF이며, 텍스트 레이어에서 제목, 공식, 예제 문장을 추출할 수 있음을 확인함.
- [x] 예제 풀이/도식 확인을 위해 페이지별 렌더링 이미지를 생성해 필요한 페이지를 시각적으로 판독함.
- [x] 판독 경로: `lecture_slide.pdf` = `텍스트 활용 + 렌더링 보조 확인`.
- [x] 외부 자료는 본문 범위 확장용으로 사용하지 않음.
- [x] 렌더링 캐시: `output/_render_cache/고체역학/8-2/`에 보관함.

## 작성 계획
- [x] Phase 1: manifest 재생성
- [x] Phase 2: PDF 텍스트 레이어 및 렌더링 판독
- [x] Phase 3: 페이지 판독 기록 작성
- [x] Phase 4: 본문 마크다운 작성
- [x] Phase 5: 구조 및 입력 커버리지 검증

## 페이지 판독 기록
- `lecture_slide.pdf` p.1: `Solid Mechanics`, `Chapter 5` 표지 확인. 본문 주제는 비틀림으로 정리함.
- `lecture_slide.pdf` p.2: Chapter 5 목차 확인. 비틀림(Torsion), 탄성 변형, 정정 불능계, 열응력 항목 중 현재 PDF에는 비틀림 중심 내용이 제시됨.
- `lecture_slide.pdf` p.3: 원형 축의 비틀림, 토크의 정의, 작은 비틀림각 가정 확인.
- `lecture_slide.pdf` p.4~5: 전단변형률 정의, 호 길이 관계, 반지름 방향 선형 분포 확인.
- `lecture_slide.pdf` p.6: 비틀림 공식 도출, $\tau = T\rho/J$, $\tau_{\max}=Tc/J$, 변수 정의 확인.
- `lecture_slide.pdf` p.7~8: 실축과 중공축의 극관성모멘트 공식 확인.
- `lecture_slide.pdf` p.9~10: Example 5.1, 외측 영역이 저항하는 토크 비율 문제 확인. p.10은 제목만 있고 세부 풀이 없음.
- `lecture_slide.pdf` p.11~12: Example 5.2, 단면 a-a의 A/B 전단응력 문제와 결과 도식 확인.
- `lecture_slide.pdf` p.13: 동력 전달, 일-동력-토크 관계, $P=T\omega=2\pi fT$ 확인.
- `lecture_slide.pdf` p.14: 축 설계, 허용전단응력 기준과 실축/중공축 설계식 확인.
- `lecture_slide.pdf` p.15~16: Example 5.4, 3750 W, 175 rpm, $\tau_{\text{allow}}=100$ MPa 조건 확인. p.16은 제목만 있고 세부 풀이 없음.
- `lecture_slide.pdf` p.17~19: 비틀림각 도출, 일반 적분식, 일정 단면/일정 토크 공식, 부호 규약 도식 확인.
- `lecture_slide.pdf` p.20~21: Example 5.6, 기어 연결 축의 비틀림각 문제와 도식 확인. p.21은 제목만 있고 세부 풀이 없음.
- `lecture_slide.pdf` p.22~23: Example 5.7, 토양 저항을 받는 매입 기둥 문제와 등분포 비틀림 저항 도식 확인. p.23 제목은 `Example 5.6`으로 표시되어 있으나 문맥상 Example 5.7 보조 도식으로 판단함.
- `lecture_slide.pdf` p.24: 양단 고정 축의 정정 불능 비틀림, 평형조건과 적합조건, 반력 토크 분배식 확인.
- `lecture_slide.pdf` p.25~26: Example 5.8, 양단 고정 실축 반력 토크 문제 확인. p.26은 제목만 있고 세부 풀이 없음.
- `lecture_slide.pdf` p.27~28: Example 5.9, 강재/황동 복합축의 전단응력 분포 문제 확인. p.28은 제목만 있고 세부 풀이 없음.

## 입력 커버리지 검증
- [x] `lecture_slide.pdf` 전체 28쪽을 검토함.
- [x] p.1~2의 표지/목차는 본문 구조와 범위 기록에 반영함.
- [x] p.3~8의 비틀림 기본 가정, 전단변형률, 비틀림 공식, 극관성모멘트 공식을 본문에 반영함.
- [x] p.9~12의 Example 5.1, 5.2는 문제 의도와 확인 가능한 결과를 본문 예제로 반영함.
- [x] p.13~16의 동력 전달과 축 설계식을 본문에 반영함.
- [x] p.17~23의 비틀림각 공식과 Example 5.6, 5.7 도식 흐름을 본문에 반영함.
- [x] p.24~28의 정정 불능 축과 Example 5.8, 5.9의 해석 틀을 본문에 반영함.
- [x] 제목만 있고 풀이가 없는 슬라이드는 새 풀이를 창작하지 않고 문제 조건 또는 풀이 절차 수준으로 제한함.
- [x] 구조 검증 결과와 입력 커버리지 검증 결과를 분리해 최종 보고에 반영함.

## 범위 기록
- [확정] `lecture_slide.pdf`만 제공되어 전체 PDF를 본문 범위 기준으로 사용함.
- [제외] `lecture_stt.txt`, `handwritten_note.pdf`는 폴더에 없으므로 이번 manifest 입력에서 제외함.
- [제외] p.10, p.16, p.21, p.26, p.28은 제목 외 실질 풀이가 없어 본문에서 풀이 창작을 하지 않음.
- [보정] p.23의 제목은 `Example 5.6`으로 보이나 p.22의 토양 저항 예제 보조 도식으로 문맥상 정리함.

## 검증 로그
- `git status --short`
- `git diff --stat`
- `Get-ChildItem -Force "inbox/고체역학/8-2"`
- `Get-Content -Raw -Encoding UTF8 "agent_rules/input_guide.md"`
- `Get-Content -Raw -Encoding UTF8 "agent_rules/note_creation_guide.md"`
- `Get-Content -Raw -Encoding UTF8 "agent_rules/validation_guide.md"`
- `Get-Content -Raw -Encoding UTF8 "agent_rules/memory_management_guideline.md"`
- `Get-Content -Raw -Encoding UTF8 "agent_rules/web_cross_validation_guide.md"`
- `conda run -n pdfkit310 python --version`
- `conda run -n pdfkit310 python -c "... fitz.open(...); page text extraction ..."`
- `conda run -n pdfkit310 python -c "... render lecture_slide.pdf pages ..."`
- 수동 구조 검증: H1 1개, 번호형 H2 사용, H2 종료 구분선 확인, 블록 수식 단독 줄 배치 확인
- 입력 커버리지 검증: `lecture_slide.pdf` p.1~p.28 판독 기록과 본문 반영/제외 사유 대조
- 제한적 웹 교차 검증: 수행하지 않음. 원본 슬라이드 공식과 보편 표기가 명확하고 추가 의심 용어 없음.

## 잔여 이슈
- 치명적 잔여 이슈: 없음.
- 비치명적 판독 이슈: 일부 예제 풀이 슬라이드가 제목만 있어 최종 수치 풀이를 새로 창작하지 않음.
- 보정 기록: p.23 제목 표기(`Example 5.6`)는 문맥상 Example 5.7 보조 도식으로 처리함.
- 환경 이슈: 루트 `schemas/` 폴더는 없고 `agent_rules/schemas/`만 확인되어 프로젝트 내 실제 위치의 JSON 규칙을 기준으로 검증함.
- 환경 이슈: `conda run`에서 여러 줄 `python -c` 및 CP949 출력 인코딩 문제가 발생해 한 줄 명령과 ASCII escape 출력으로 전환함.
- 변경 상태 이슈: 작업 전부터 tracked 변경 및 `inbox/`, `output/` untracked 상태가 존재함.
