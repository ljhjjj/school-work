# Issue 13. `lecture_slide.pdf` 단독 입력 시 전체 PDF를 `primary_scope_anchor`로 처리

## 문제

현재 파일 조합별 본문 범위 규칙에는 다음 경우가 정의되어 있다.

```text
1. STT + 손글씨 + 강의자료 PDF
   → STT가 본문 범위 기준

2. 필기 PDF + 강의자료 PDF
   → 필기 PDF가 본문 범위 기준

3. 자료 PDF 위에 필기만 있는 경우
   → 자료 PDF 구조 + 필기 보충
```

하지만 `lecture_slide.pdf`만 제공된 경우가 명시되어 있지 않다.

이 상태에서는 슬라이드 PDF만 들어온 작업에서 Codex가 해당 PDF를 본문 범위 기준으로 삼아야 하는지, 단순 참고 자료로만 봐야 하는지 판단하기 어렵다.

## 수정 대상

- `agent_rules/input_guide.md`
    
- `agent_rules/input_acquisition_guide.md`
    
- 필요 시 `agent_rules/validation_guide.md`
    
- 필요 시 `agent_rules/schemas/validation_rules.json`
    
- 필요 시 manifest 예시가 있는 문서
    

## 요구 사항

### 1. `input_guide.md`에 slide-only 조합 추가

`input_guide.md`의 “파일 조합별 본문 범위 및 검증 우선순위” 섹션 아래에 다음 규칙을 추가한다.

```md
#### 4. 강의자료 PDF만 제공되는 경우

- `lecture_slide.pdf`만 제공된 경우, 해당 PDF 전체를 본문 범위의 기준으로 삼는다.
- 이 경우 `lecture_slide.pdf`의 role은 `primary_scope_anchor`로 취급한다.
- 슬라이드의 제목, 목차, 섹션 순서, 공식, 그림, 예제 번호를 본문 구조의 기준으로 사용한다.
- 모든 슬라이드는 본문 반영 대상이다.
- 단, 슬라이드가 목차, 표지, 간지, 참고 이미지처럼 본문 설명 가치가 낮은 경우에는 별도 H2/H3로 과도하게 확장하지 않고, 필요한 경우 짧게 요약하거나 PLAN에 `[제외]` 사유를 기록한다.
- 슬라이드에 문장 설명이 부족한 경우, 슬라이드에 표시된 공식·그림·키워드를 바탕으로 전공적으로 자연스러운 설명을 보충한다.
- 보충 설명은 슬라이드 범위 안에서만 작성하며, 슬라이드에 없는 새로운 예제, 공식, 심화 주제는 임의로 추가하지 않는다.
- 판독 불가한 그림, 수식, 표는 본문에 단정적으로 넣지 않고 `[필기 판독 불확실]`, `[확인 필요]`, `[수동 입력 필요]` 중 적절한 태그로 PLAN 또는 최종 리포트에 기록한다.
- 외부 웹 자료는 본문 범위 확장용으로 사용하지 않고, 용어·공식 표기 확인에만 제한적으로 사용한다.
```

### 2. manifest 예시에 slide-only 입력 추가

`input_acquisition_guide.md`에 `lecture_slide.pdf`만 있는 경우의 manifest 예시를 추가한다.

```json
{
  "status": "pending",
  "task_priority": 40,
  "subject": "열역학",
  "week": "8",
  "session": "1",
  "task_type": "new_note",
  "input_root": "inbox/열역학/8-1",
  "output_note": "output/열역학/열역학 8-1.md",
  "plan_file": "output/열역학/열역학_8-1_PLAN.md",
  "sources": [
    {
      "path": "inbox/열역학/8-1/lecture_slide.pdf",
      "type": "ppt_pdf",
      "role": "primary_scope_anchor",
      "priority": 1,
      "scope": "entire_pdf"
    }
  ]
}
```

핵심 필드는 반드시 아래처럼 유지한다.

```json
"role": "primary_scope_anchor",
"scope": "entire_pdf"
```

### 3. PLAN 기록 방식 추가

슬라이드만 제공된 경우 PLAN에 다음과 같은 입력 판정 기록을 남기도록 한다.

```md
## 입력 판정

- `lecture_slide.pdf`만 제공되어 해당 PDF 전체를 본문 범위 기준(primary_scope_anchor)으로 사용함.
- 텍스트 레이어를 1회 점검하고, 유효하면 텍스트 추출 결과를 활용함.
- 텍스트 추출이 불충분하면 페이지별 렌더링/OCR로 전환함.
- 판독 경로: `lecture_slide.pdf` = `텍스트 활용` 또는 `렌더링/OCR 전환`.
- 외부 자료는 본문 범위 확장용으로 사용하지 않고 용어·공식 표기 확인용으로만 사용함.
```

페이지 판독 기록은 전체 페이지를 대상으로 하되, 페이지가 많으면 구간 단위로 묶어 기록하도록 한다.

```md
## 페이지 판독 기록

- `lecture_slide.pdf` p.1~3: 강의 제목, 일정, 전체 목차 확인
- `lecture_slide.pdf` p.4~7: 핵심 개념과 주요 예시 확인
- `lecture_slide.pdf` p.8~12: 공식, 그림, 표, 계산 흐름 확인
- `lecture_slide.pdf` p.13~17: 응용 개념과 다이어그램 확인
- `lecture_slide.pdf` p.18: 예제 문제 및 풀이 흐름 확인
```

### 4. 검증 규칙에 slide-only 커버리지 반영

`validation_guide.md` 또는 `validation_rules.json`에 입력 커버리지 검증 기준을 보완한다.

slide-only 조합에서는 다음을 검증 대상으로 삼는다.

```text
- lecture_slide.pdf 전체 페이지가 검토되었는가
- 각 슬라이드 또는 슬라이드 구간이 페이지 판독 기록에 남아 있는가
- 최종 본문이 슬라이드의 제목, 섹션 순서, 공식, 그림, 예제 흐름을 반영하는가
- 제외한 슬라이드가 있다면 PLAN 또는 최종 리포트에 제외 사유가 기록되었는가
- 외부 자료가 본문 범위 확장용으로 사용되지 않았는가
```

## 최종 정책

파일 조합별 본문 범위 기준은 아래처럼 정리한다.

```md
- `lecture_slide.pdf`만 있음:
  - `lecture_slide.pdf` 전체 = 본문 범위
  - role = `primary_scope_anchor`
  - scope = `entire_pdf`
  - 모든 슬라이드 또는 슬라이드 구간을 커버리지 검증 대상으로 삼음

- `handwritten_note.pdf` + `lecture_slide.pdf`:
  - `handwritten_note.pdf` = 본문 범위
  - `lecture_slide.pdf` = 보조 확인

- `STT TXT` + `handwritten_note.pdf` + `lecture_slide.pdf`:
  - `STT TXT` = 본문 범위
  - 나머지는 범위 안에서 병합/검증
```

## 완료 기준

- `lecture_slide.pdf`만 제공된 경우가 파일 조합 규칙에 명시되어 있다.
    
- slide-only 조합에서 `lecture_slide.pdf`의 role이 `primary_scope_anchor`로 정의되어 있다.
    
- manifest 예시에 `"scope": "entire_pdf"`가 포함되어 있다.
    
- PLAN 입력 판정과 페이지 판독 기록 예시가 추가되어 있다.
    
- slide-only 작업에서 전체 PDF 커버리지 검증을 수행하도록 검증 규칙이 보완되어 있다.
    
- 외부 웹 자료는 본문 범위 확장용이 아니라 용어·공식 표기 확인용으로만 제한되어 있다.
    
- JSON 파일을 수정한 경우 파싱 검증을 통과해야 한다.
    

## Codex에 같이 붙일 공통 지시

```text
수정 전 관련 파일을 먼저 읽고 현재 섹션 구조를 확인할 것.
기존 파일 조합 규칙의 표현 방식과 문체를 최대한 유지할 것.
slide-only 조합만 추가하고, 기존 STT/필기/PDF 조합의 우선순위는 불필요하게 바꾸지 말 것.
JSON 파일을 수정한 경우 PowerShell ConvertFrom-Json 또는 동등한 JSON 파서로 검증할 것.
작업 후 git status --short와 변경 파일 요약을 보고할 것.
```