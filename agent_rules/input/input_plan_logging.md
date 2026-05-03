<!-- Guide Tier: P2 -->
<!-- Purpose: PLAN 입력 판정, 판독 경로, 페이지 판독 기록 방식 정의 -->

# Input_Plan_Logging

## 5. PLAN 기록 형식

### 5.1 입력 판정

PDF 판독 전략을 정한 경우 PLAN에 `## 입력 판정` 섹션을 두고 다음 형식으로 기록한다.

- `handwritten_note.pdf`는 손글씨 문서로 판단되어 텍스트 직접 추출을 생략하고 페이지 렌더링/OCR로 판독함.
- `lecture_slide.pdf`는 텍스트 레이어를 1회 점검했으나 판독 효율이 낮아 페이지 렌더링/OCR로 전환함.
- 텍스트 추출 실패는 오류가 아니라 렌더링/OCR fallback 조건 충족으로 기록함.
- 판독 경로: `handwritten_note.pdf` = `텍스트 추출 생략`, `lecture_slide.pdf` = `렌더링/OCR 전환`.
- 렌더링 방식: 페이지별 렌더링을 사용함. Edge screenshot은 기본 판독 경로로 사용하지 않음.
- 렌더링 캐시: 기본 모드에서는 작업 완료 후 즉시 삭제하지 않고 24시간 보관 대상으로 둔다. 디버그 모드에서는 `output/_render_cache/[과목명]/[주차]-[차시]/`에 보관한다.

`lecture_slide.pdf`만 제공된 경우 PLAN의 `## 입력 판정` 섹션에 다음 내용을 기록한다.

- `lecture_slide.pdf`만 제공되어 해당 PDF 전체를 본문 범위 기준(`primary_scope_anchor`)으로 사용함.
- 텍스트 레이어를 1회 점검하고, 유효하면 텍스트 추출 결과를 활용함.
- 텍스트 추출이 불충분하면 페이지별 렌더링/OCR로 전환함.
- 판독 경로: `lecture_slide.pdf` = `텍스트 활용` 또는 `렌더링/OCR 전환`.
- 외부 자료는 본문 범위 확장용으로 사용하지 않고 용어·공식 표기 확인용으로만 사용함.

### 5.2 페이지 판독 기록

PDF 또는 이미지 입력을 판독한 경우, Full PLAN에서는 PLAN에 `## 페이지 판독 기록` 섹션을 두고 페이지별 근거를 남긴다.

- Mini PLAN에서는 페이지 판독 기록은 선택 사항이다. 필요한 경우에만 `p.1~5: 핵심 개념 확인` 형태로 1줄 요약한다.
- Full PLAN에서는 페이지 단위 판독 기록을 권장하되, 연속 구간은 묶어 기록한다.
- 각 항목은 `파일명 p.번호: 핵심 개념, 공식, 도식, 용어, 사용 목적` 형식으로 기록한다.
- 손글씨/필기 PDF는 본문 반영 근거인지, 판독 불확실 항목인지 구분한다.
- 슬라이드 PDF는 본문 확장용인지, 용어·공식 보조 확인용인지 구분한다.
- Full PLAN에서는 `lecture_slide.pdf`만 제공된 경우 모든 페이지를 판독 기록 대상으로 삼고, 페이지가 많으면 연속 구간 단위로 묶어 기록할 수 있다.
- 예: `handwritten_note.pdf p.1: 제2법칙, 열효율, COP, 열저장조, Kelvin-Planck, Clausius`
- 예: `handwritten_note.pdf p.2: Kelvin-Planck statement, Clausius statement, 두 명제의 동치성 논증`
- 예: `handwritten_note.pdf p.3: 도식 논증, reversible/reversed process 구분 및 반영 필요 항목`
- 예: `lecture_slide.pdf p.10~12: 열저장조, 열효율, COP 용어·공식 보조 확인`
- 예: `lecture_slide.pdf p.1~3: 강의 제목, 일정, 전체 목차 확인`
- 예: `lecture_slide.pdf p.4~7: 핵심 개념과 주요 예제 확인`
- 예: `lecture_slide.pdf p.8~12: 공식, 그림, 계산 흐름 확인`
- 예: `lecture_slide.pdf p.13~18: 적용 개념, 다이어그램, 예제 문제 확인`
