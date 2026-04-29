# Input Acquisition Guide

## 1. 목적
사용자가 매번 채팅으로 파일 위치를 설명하지 않아도, Codex가 정해진 입력 폴더와 manifest를 기준으로 작업 대상을 자동 발견하도록 한다.

## 2. 기본 입력 루트
- 기본 입력 루트는 `inbox/`이다.
- 작업 단위 폴더는 `inbox/[과목명]/[주차]-[차시]/` 형식을 따른다.

## 3. Manifest 우선 원칙
- 각 작업 폴더에는 가능한 경우 `input_manifest.json`을 둔다.
- Codex는 파일명 추론보다 manifest를 우선한다.

## 4. 처리 상태
- `pending`: 처리 대기
- `in_progress`: 처리 중
- `completed`: 처리 완료
- `blocked`: 데이터 누락 또는 판독 불가
- `failed`: 시스템/검증 오류

## 5. Manifest 누락 시 Fallback
- manifest가 없으면 폴더명과 파일 확장자로 과목/주차/차시/파일 유형을 추론한다.
- 추론 불가능할 때만 사용자에게 질문한다.

## 6. 완료 후 처리
- 완료된 입력 폴더는 `archive/`로 이동하지 않고, manifest 상태만 `completed`로 바꾼다.
- 원본 파일은 보존한다.

## 7. input_manifest.json Manifest 예시

### Manifest 예시 A: 순수 손글씨 PDF + 음성 TXT + 강의자료 PDF
```json
{
  "status": "pending",
  "task_priority": 10,
  "subject": "고체역학",
  "week": "3",
  "session": "1",
  "task_type": "new_note",
  "input_root": "inbox/고체역학/3-1",
  "output_note": "고체역학/고체역학 3-1.md",
  "plan_file": "고체역학/고체역학_3-1_PLAN.md",
  "sources": [
    {
      "path": "inbox/고체역학/3-1/lecture_stt.txt",
      "type": "stt_txt",
      "role": "primary_scope_anchor",
      "priority": 1
    },
    {
      "path": "inbox/고체역학/3-1/handwritten_note.pdf",
      "type": "note_pdf",
      "role": "bounded_merge_source",
      "priority": 2
    },
    {
      "path": "inbox/고체역학/3-1/lecture_slide.pdf",
      "type": "ppt_pdf",
      "role": "verification_only",
      "priority": 3
    }
  ]
}
```

### Manifest 예시 B: 필기 PDF + 강의자료 PDF
```json
{
  "status": "pending",
  "task_priority": 20,
  "subject": "열역학",
  "week": "4",
  "session": "2",
  "task_type": "new_note",
  "input_root": "inbox/열역학/4-2",
  "output_note": "열역학/열역학 4-2.md",
  "plan_file": "열역학/열역학_4-2_PLAN.md",
  "sources": [
    {
      "path": "inbox/열역학/4-2/annotated_note.pdf",
      "type": "note_pdf",
      "role": "primary_scope_anchor",
      "priority": 1
    },
    {
      "path": "inbox/열역학/4-2/lecture_slide.pdf",
      "type": "ppt_pdf",
      "role": "verification_only",
      "priority": 2
    }
  ]
}
```

### Manifest 예시 C: 자료 PDF 위에 필기만 있는 경우
```json
{
  "status": "pending",
  "task_priority": 30,
  "subject": "동역학",
  "week": "5",
  "session": "1",
  "task_type": "new_note",
  "input_root": "inbox/동역학/5-1",
  "output_note": "동역학/동역학 5-1.md",
  "plan_file": "동역학/동역학_5-1_PLAN.md",
  "sources": [
    {
      "path": "inbox/동역학/5-1/lecture_slide.pdf",
      "type": "ppt_pdf",
      "role": "structure_anchor",
      "priority": 1
    },
    {
      "path": "inbox/동역학/5-1/annotated_slide.pdf",
      "type": "annotated_pdf",
      "role": "primary_scope_anchor",
      "priority": 2
    },
    {
      "path": "inbox/동역학/5-1/handwriting_layer.png",
      "type": "handwriting_image",
      "role": "bounded_merge_source",
      "priority": 3
    }
  ]
}
```

### 우선순위 필드 정의
- `task_priority`: 여러 pending 작업 중 어떤 작업을 먼저 처리할지 정하는 최상위 작업 선택 우선순위이다. 값이 낮을수록 먼저 처리한다.
- `sources[].priority`: 선택된 하나의 작업 안에서 입력 자료를 읽는 순서로만 사용한다. 본문 범위 우선순위나 작업 선택 우선순위로 사용하지 않는다.

### 파일 역할(Role) 정의

| role                   | 의미 |
| ---------------------- | --- |
| `primary_scope_anchor` | 본문에 포함할 주제와 범위를 최종 결정하는 기준 자료 |
| `bounded_merge_source` | `primary_scope_anchor`에서 확인된 범위 안에서만 병합 가능한 보조 자료 |
| `structure_anchor`     | 목차, 슬라이드 순서, 예제 번호 등 문서 구조를 잡는 기준 자료 |
| `verification_only`    | 용어, 공식, 기호, STT/OCR 오류 검증에만 사용하고 본문 범위를 확장하지 않는 자료 |

## 8. 작업 선택 우선순위
1. 사용자가 과목/주차/차시를 명시한 작업을 최우선으로 선택한다.
2. 그다음 `status: pending`인 manifest를 선택한다.
3. pending 작업이 여러 개면 manifest 최상위 `task_priority` 값이 낮은 작업을 우선한다.
4. manifest 최상위 `task_priority`가 없으면 최신 수정 시간이 가장 오래된 작업부터 처리한다.
5. 그래도 불명확하면 사용자에게 한 번만 질문한다.

## 9. 상태 전이 규칙
- 작업 시작 직후 `pending` → `in_progress`
- 정상 완료 후 `in_progress` → `completed`
- 데이터 누락/판독 불가로 사용자 입력이 필요한 경우 `blocked`
- 시스템 오류, 저장 실패, 검증 실패로 계속할 수 없는 경우 `failed`
- `blocked` 또는 `failed` 전환 시 PLAN에 재개 지점을 기록한다.
