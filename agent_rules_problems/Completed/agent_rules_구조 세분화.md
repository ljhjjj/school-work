# agent_rules 구조 세분화 리팩토링 Issue 요청서

  

> 목적: `agent_rules/`의 큰 가이드 파일을 작업 단계별 소형 파일로 분리하여 Codex/Kimi가 필요한 규칙만 읽도록 만든다.  

> 범위: **루트 `AGENTS.md` 수정은 제외한다.**  

> 권장 사용 방식: **한 번에 하나의 Issue만** 붙여서 처리한다.  

> 원칙: 서로 의존성이 큰 라우팅/참조 갱신은 뒤로 미루고, 독립적으로 분리 가능한 작업부터 진행한다.  

> 권장 처리 순서: Issue 1 → 2 → 3 → 4 → 5 → 6 → 7 → 8 → 9 → 10 → 11 → 12 → 13 → 14

  

---

  

# 공통 지시

  

```text

수정 전 관련 파일을 먼저 읽고 현재 섹션 구조를 확인할 것.

루트 AGENTS.md는 수정하지 말 것.

기존 규칙의 표현 방식과 문체를 최대한 유지할 것.

한 Issue만 처리하고, 다른 Issue의 내용은 불필요하게 섞지 말 것.

Issue 요구사항과 충돌하지 않는 기존 P0 안전 정책은 절대 삭제하거나 약화하지 말 것.

규칙을 새 파일로 옮길 때는 의미를 바꾸지 말고, 원문을 최대한 보존할 것.

기존 문구를 제거하는 경우, 제거 사유와 대체 위치를 작업 보고에 명시할 것.

새 파일을 만들 경우 UTF-8로 저장할 것.

JSON 파일을 수정한 경우 PowerShell ConvertFrom-Json 또는 동등한 JSON 파서로 검증할 것.

작업 후 git status --short와 변경 파일 요약을 보고할 것.

수정 사항이 기존 안전 정책(위험 명령 제한, 기존 변경 보호, UTF-8 인코딩, primary_scope_anchor 준수)을 약화시키지 않도록 확인할 것.

  

토큰 절약 및 파일 접근 제한:

- 수정 대상 파일만 읽을 것.

- 같은 파일을 전체 ReadFile로 2회 이상 읽지 말 것.

- 재확인이 필요하면 전체 ReadFile 대신 검색 또는 해당 섹션만 확인할 것.

- StrReplaceFile이 실패하면 같은 방식으로 2회 이상 재시도하지 말 것.

- 치환이 어려우면 섹션 시작/끝 기준으로 한 번에 교체할 것.

- 임시 Python 스크립트는 최대 1개만 만들 것.

- 전체 파일 내용을 최종 보고에 출력하지 말 것.

- 다른 Issue의 요구사항은 분석하지 말고 수정하지 말 것.

```

  

---

  

# 처리 단계 개요

  

## 독립 분리 우선 단계

  

먼저 긴 예시, PLAN 정책, 입력 태그, PDF 판독 전략처럼 비교적 독립적인 섹션을 분리한다.

  

- Issue 1: 디렉터리 뼈대 생성

- Issue 2: PLAN 예시 분리

- Issue 3: PLAN 언어 정책 분리

- Issue 4: Mini/Full PLAN 정책 분리

- Issue 5: Blocker/재개 정책 분리

- Issue 6: 불확실성 태그 분리

- Issue 7: PDF 판독 전략 분리

- Issue 8: primary_scope_anchor 범위 규칙 분리

- Issue 9: PLAN 입력 판정/페이지 판독 기록 분리

- Issue 10: 이미지 트러블슈팅 분리

  

## 중간 연결 단계

  

입력/PLAN 분리가 끝난 뒤 작성/검증/실행 가이드를 분리한다.

  

- Issue 11: note_creation_guide 분리

- Issue 12: validation_guide 분리

- Issue 13: codex_execution_guide 분리

  

## 최종 연결 단계

  

마지막에 기존 대형 파일을 얇은 인덱스 또는 라우팅 문서로 정리한다. 루트 `AGENTS.md`는 수정하지 않는다.

  

- Issue 14: 기존 대형 가이드의 중복 제거 및 내부 참조 정리

  

---

  

# Issue 1. 세분화 디렉터리 뼈대 생성

  

## 문제

  

현재 `agent_rules/`의 주요 규칙이 대형 Markdown 파일에 집중되어 있어, 작업 유형별로 필요한 규칙만 읽기 어렵습니다. 세부 규칙을 분리하려면 먼저 안정적인 디렉터리 구조가 필요합니다.

  

## 수정 대상

  

- `agent_rules/`

  

## 요구 사항

  

### 1. 하위 디렉터리 생성

  

다음 디렉터리를 생성합니다.

  

```text

agent_rules/execution/

agent_rules/input/

agent_rules/plan/

agent_rules/writing/

agent_rules/validation/

agent_rules/web/

agent_rules/examples/

```

  

### 2. README 또는 index 파일은 아직 만들지 않음

  

이 Issue에서는 디렉터리만 생성합니다.

  

- 새 규칙 파일 작성 금지

- 기존 가이드 파일 수정 금지

- 루트 `AGENTS.md` 수정 금지

  

## 완료 기준

  

- [ ] 지정된 하위 디렉터리가 모두 생성되었다.

- [ ] 기존 규칙 파일은 수정되지 않았다.

- [ ] 루트 `AGENTS.md`는 수정되지 않았다.

- [ ] git status --short에 새 디렉터리 또는 placeholder 파일 상태가 명확히 표시된다.

  

## 주의

  

Git은 빈 디렉터리를 추적하지 않으므로 필요한 경우 각 디렉터리에 `.gitkeep`을 둘 수 있다. 단, `.gitkeep` 외의 규칙 파일은 이 Issue에서 만들지 않는다.

  

---

  

# Issue 2. PLAN 예시를 examples/로 분리

  

## 문제

  

`memory_management_guideline.md`에는 실제 규칙과 긴 PLAN 예시가 함께 들어 있습니다. 예시는 유용하지만 매 작업마다 읽을 필요가 없으며, 규칙 파일의 크기를 크게 만듭니다.

  

## 수정 대상

  

- `agent_rules/memory_management_guideline.md`

- `agent_rules/examples/plan_examples.md`

  

## 요구 사항

  

### 1. PLAN 예시 파일 생성

  

`memory_management_guideline.md`의 예시 블록을 새 파일로 이동합니다.

  

대상 예시:

  

```text

--- ( [과목명]_[주차]-[차시]_PLAN.md 내부 예시 포맷 ) ---

```

  

이 표식부터 문서 끝까지의 예시 영역을 `agent_rules/examples/plan_examples.md`로 이동합니다.

  

### 2. 원래 위치에는 짧은 참조만 남김

  

`memory_management_guideline.md`의 예시 위치에는 다음과 같은 참조 문구만 남깁니다.

  

```md

## [P2] PLAN 예시

  

상세 PLAN 예시는 `agent_rules/examples/plan_examples.md`를 참조한다.

예시는 형식이 불명확하거나 사용자가 예시 확인을 요청한 경우에만 읽는다.

```

  

### 3. 의미 변경 금지

  

- 예시 내용의 의미를 바꾸지 않습니다.

- 예시 안의 체크박스, 이슈, 재개 지점 표현은 가능한 그대로 보존합니다.

- 이 Issue에서는 PLAN 정책 자체를 수정하지 않습니다.

  

## 완료 기준

  

- [ ] `agent_rules/examples/plan_examples.md`가 생성되었다.

- [ ] 기존 PLAN 예시가 새 파일로 이동되었다.

- [ ] `memory_management_guideline.md`에는 짧은 참조만 남았다.

- [ ] PLAN 정책 본문은 의미 변경 없이 유지되었다.

- [ ] 루트 `AGENTS.md`는 수정되지 않았다.

  

---

  

# Issue 3. PLAN 언어 정책을 plan/plan_language_policy.md로 분리

  

## 문제

  

PLAN 언어 정책은 독립적으로 참조 가능한 규칙입니다. 현재는 `memory_management_guideline.md` 상단에 있어 PLAN 템플릿이나 실행 루프를 읽지 않아도 되는 작업에서도 함께 읽히게 됩니다.

  

## 수정 대상

  

- `agent_rules/memory_management_guideline.md`

- `agent_rules/plan/plan_language_policy.md`

  

## 요구 사항

  

### 1. 새 파일 생성

  

`memory_management_guideline.md`의 `PLAN Language Policy` 섹션을 다음 파일로 이동합니다.

  

```text

agent_rules/plan/plan_language_policy.md

```

  

새 파일에는 다음 내용을 포함합니다.

  

- PLAN files may mix English and Korean.

- 체크박스, Phase 이름, task state, technical workflow log는 영어 가능

- 과목 주제, 개념 해석, 판독 메모, unresolved issue, user-facing confirmation note는 한국어 가능

- 최종 강의노트 언어는 `note_creation_guide.md`의 output language policy를 따른다는 원칙

- 기술적 워크플로우는 영어, 주제/해석/판독 메모는 한국어라는 기본값

- 한 섹션 안에서 불필요한 언어 반복 혼용 금지

- 사용자 언어 요청 우선

  

### 2. 원래 위치에는 참조 문구만 남김

  

`memory_management_guideline.md` 상단에는 다음처럼 축약합니다.

  

```md

## [P1] PLAN Language Policy

  

PLAN 언어 기본값은 `agent_rules/plan/plan_language_policy.md`를 따른다.

```

  

### 3. 의미 변경 금지

  

언어 정책의 의미를 바꾸지 않습니다.

  

## 완료 기준

  

- [ ] `agent_rules/plan/plan_language_policy.md`가 생성되었다.

- [ ] PLAN 언어 정책이 새 파일에 보존되었다.

- [ ] `memory_management_guideline.md`에는 참조 문구만 남았다.

- [ ] 최종 강의노트 언어 정책과 PLAN 언어 정책의 구분이 유지되었다.

- [ ] 루트 `AGENTS.md`는 수정되지 않았다.

  

---

  

# Issue 4. Mini/Full PLAN 정책을 plan/mini_full_plan.md로 분리

  

## 문제

  

Mini PLAN / Full PLAN 정책은 PLAN 관리의 핵심이지만, 실행 루프와 blocker 처리 규칙과 한 파일에 섞여 있습니다. 작은 작업에서는 Mini/Full 판단만 필요하므로 이 섹션을 독립 파일로 분리하는 것이 좋습니다.

  

## 수정 대상

  

- `agent_rules/memory_management_guideline.md`

- `agent_rules/plan/mini_full_plan.md`

  

## 요구 사항

  

### 1. 새 파일 생성

  

`memory_management_guideline.md`의 다음 섹션을 새 파일로 이동합니다.

  

```text

## [P1] 2. PLAN 템플릿 종류

```

  

새 파일 경로:

  

```text

agent_rules/plan/mini_full_plan.md

```

  

포함할 내용:

  

- Mini PLAN 적용 기준

- Mini PLAN 필수 섹션

- Full PLAN 적용 기준

- Full PLAN 필수 섹션

- Mini PLAN / Full PLAN 체크박스 관리 차이와 연결되는 설명이 있는 경우 함께 포함

  

### 2. 원래 위치에는 참조 문구만 남김

  

`memory_management_guideline.md`에는 다음처럼 남깁니다.

  

```md

## [P1] 2. PLAN 템플릿 종류

  

Mini PLAN과 Full PLAN의 적용 기준 및 필수 섹션은 `agent_rules/plan/mini_full_plan.md`를 따른다.

```

  

### 3. 중첩 코드블록 보존

  

Mini PLAN 예시의 Markdown fence가 깨지지 않도록 합니다.

  

## 완료 기준

  

- [ ] `agent_rules/plan/mini_full_plan.md`가 생성되었다.

- [ ] Mini PLAN / Full PLAN 적용 기준이 새 파일에 보존되었다.

- [ ] Mini PLAN 필수 섹션 예시가 깨지지 않는다.

- [ ] `memory_management_guideline.md`에는 참조 문구만 남았다.

- [ ] 루트 `AGENTS.md`는 수정되지 않았다.

  

---

  

# Issue 5. Blocker 및 재개 정책을 plan/blocker_and_resume.md로 분리

  

## 문제

  

`memory_management_guideline.md`의 TROUBLESHOOT, blocked 전환, 시스템/실행 이슈, 재개 지점 기록 규칙은 PLAN 템플릿 자체와는 다른 성격의 규칙입니다. 긴 작업 재개가 필요한 경우에만 읽히도록 분리하는 것이 좋습니다.

  

## 수정 대상

  

- `agent_rules/memory_management_guideline.md`

- `agent_rules/plan/blocker_and_resume.md`

  

## 요구 사항

  

### 1. 새 파일 생성

  

다음 내용을 새 파일로 이동합니다.

  

```text

agent_rules/plan/blocker_and_resume.md

```

  

포함할 내용:

  

- blocked 전환 조건

- 질문해야 하는 데이터 이슈

- 심각한 데이터 누락 또는 핵심 전제 판독 불가 시 처리

- 재요청 기본 2회 + 핵심 전제 추가 1회 + 총 3회 제한

- `[핵심 전제: 추가 재요청 1회]` 기록 규칙

- 범위 불확실성 처리

- 입력 매칭 불확실성 처리

- 시스템/실행 이슈 처리

- 현재 완료 범위와 다음 재개 지점 기록

  

### 2. `memory_management_guideline.md`에는 요약과 참조만 남김

  

TROUBLESHOOT 위치에는 다음처럼 축약합니다.

  

```md

5. **TROUBLESHOOT (데이터 누락 및 시스템 예외 대기):**

   - blocked 전환, 재요청 제한, 재개 지점 기록은 `agent_rules/plan/blocker_and_resume.md`를 따른다.

```

  

### 3. Mini/Full PLAN 정책은 건드리지 않음

  

이 Issue에서는 Mini/Full PLAN 섹션을 수정하지 않습니다.

  

## 완료 기준

  

- [ ] `agent_rules/plan/blocker_and_resume.md`가 생성되었다.

- [ ] blocked, 재요청, 재개 지점 기록 규칙이 새 파일에 보존되었다.

- [ ] `memory_management_guideline.md`의 TROUBLESHOOT 섹션은 참조 중심으로 축약되었다.

- [ ] 재요청 제한 정책은 의미 변경 없이 유지되었다.

- [ ] 루트 `AGENTS.md`는 수정되지 않았다.

  

---

  

# Issue 6. 불확실성 태그를 input/uncertainty_tags.md로 분리

  

## 문제

  

`input_guide.md`에는 정상 판정/범위 관리 태그와 잔여 이슈 태그가 길게 정의되어 있습니다. 이 태그 정의는 입력 범위 규칙과 관련 있지만, 매번 전체 입력 가이드를 읽지 않아도 독립적으로 참조할 수 있습니다.

  

## 수정 대상

  

- `agent_rules/input_guide.md`

- `agent_rules/input/uncertainty_tags.md`

  

## 요구 사항

  

### 1. 새 파일 생성

  

`input_guide.md`의 다음 섹션을 새 파일로 이동합니다.

  

```text

#### 5. 충돌 및 불확실성 태그

```

  

새 파일 경로:

  

```text

agent_rules/input/uncertainty_tags.md

```

  

포함할 내용:

  

- 정상 판정/범위 관리 태그

- 잔여 이슈 태그

- 정상 판정/범위 관리 태그는 validation warning으로 잡지 않는다는 원칙

- 잔여 이슈 태그는 최종 리포트의 `[잔여 이슈]`에 포함한다는 원칙

  

### 2. 원래 위치에는 참조 문구만 남김

  

`input_guide.md`에는 다음처럼 남깁니다.

  

```md

#### 5. 충돌 및 불확실성 태그

  

태그 정의와 리포트 포함 기준은 `agent_rules/input/uncertainty_tags.md`를 따른다.

```

  

### 3. JSON과의 관계 유지

  

태그 분류가 `schemas/validation_rules.json`의 `normal_status_tags`, `residual_issue_tags`와 충돌하지 않도록 합니다. JSON은 수정하지 않습니다.

  

## 완료 기준

  

- [ ] `agent_rules/input/uncertainty_tags.md`가 생성되었다.

- [ ] 정상 판정/범위 관리 태그와 잔여 이슈 태그가 새 파일에 보존되었다.

- [ ] `input_guide.md`에는 짧은 참조만 남았다.

- [ ] `validation_rules.json`은 수정되지 않았다.

- [ ] 루트 `AGENTS.md`는 수정되지 않았다.

  

---

  

# Issue 7. PDF 판독 전략을 input/pdf_reading_strategy.md로 분리

  

## 문제

  

`input_guide.md`의 PDF 판독 전략은 파일 유형 분류, 텍스트 추출 fallback, 렌더링/OCR 전환 기준을 포함합니다. 이는 PDF가 있는 작업에서만 필요하므로 독립 파일로 분리하는 것이 좋습니다.

  

## 수정 대상

  

- `agent_rules/input_guide.md`

- `agent_rules/input/pdf_reading_strategy.md`

  

## 요구 사항

  

### 1. 새 파일 생성

  

다음 내용을 새 파일로 이동합니다.

  

```text

agent_rules/input/pdf_reading_strategy.md

```

  

포함할 내용:

  

- `.txt`, `.pdf`, `.jpg` / `.png`의 기본 파일 유형 분류 중 PDF 판독에 필요한 부분

- `.pdf 처리: 명확한 문서 유형 판별 기준`

- `PPT 형태` 판별 기준

- `노트 필기` 판별 기준

- `PDF 판독 전략: 텍스트 추출 실패 시 빠른 fallback`

- 텍스트 추출 활용/렌더링 전환/텍스트 추출 생략 기록 기준

- 손글씨 수식 중심 노트 처리 규칙 중 PDF 판독 전략과 직접 관련된 내용

  

### 2. `input_guide.md`에는 요약과 참조만 남김

  

원래 위치에는 다음처럼 축약합니다.

  

```md

- PDF 문서 유형 판별, 텍스트 추출 fallback, 페이지 렌더링/OCR 전환 기준은 `agent_rules/input/pdf_reading_strategy.md`를 따른다.

```

  

### 3. 렌더링 캐시 문구는 수정하지 않음

  

렌더링 캐시 보관 정책 충돌은 이 Issue에서 수정하지 않습니다.  

단, 분리 과정에서 기존 문구를 이동해야 한다면 원문 의미를 그대로 보존합니다.

  

## 완료 기준

  

- [ ] `agent_rules/input/pdf_reading_strategy.md`가 생성되었다.

- [ ] PDF 유형 판별 및 렌더링/OCR fallback 규칙이 새 파일에 보존되었다.

- [ ] `input_guide.md`의 해당 부분은 참조 중심으로 축약되었다.

- [ ] PDF 판독 전략의 의미가 변경되지 않았다.

- [ ] 루트 `AGENTS.md`는 수정되지 않았다.

  

---

  

# Issue 8. primary_scope_anchor 범위 규칙을 input/scope_anchor_rules.md로 분리

  

## 문제

  

`input_guide.md`에서 가장 중요한 규칙은 파일 조합별 본문 범위와 `primary_scope_anchor`입니다. 이 규칙은 강의노트 생성/수정 작업에서 핵심이므로 독립 파일로 분리해 항상 명확히 참조할 수 있어야 합니다.

  

## 수정 대상

  

- `agent_rules/input_guide.md`

- `agent_rules/input/scope_anchor_rules.md`

  

## 요구 사항

  

### 1. 새 파일 생성

  

다음 섹션을 새 파일로 이동합니다.

  

```text

### [P0] 파일 조합별 본문 범위 및 검증 우선순위(primary_scope_anchor)

```

  

새 파일 경로:

  

```text

agent_rules/input/scope_anchor_rules.md

```

  

포함할 내용:

  

- 사용자 질문 정책 중 본문 범위 대응과 관련된 부분

- 순수 손글씨 PDF + 음성 TXT + 강의자료 PDF

- 필기 PDF + 강의자료 PDF만 제공되는 경우

- 자료 PDF 위에 필기만 있는 경우

- 강의자료 PDF만 제공되는 경우

- `primary_scope_anchor`가 본문 범위를 결정한다는 원칙

- 외부 자료와 보조 자료가 본문 범위를 확장하지 않는다는 원칙

- `lecture_slide.pdf` 단독 입력 시 전체 PDF를 범위 기준으로 삼는 규칙

  

### 2. `input_guide.md`에는 참조만 남김

  

원래 위치에는 다음처럼 축약합니다.

  

```md

### [P0] 파일 조합별 본문 범위 및 검증 우선순위(primary_scope_anchor)

  

파일 조합별 본문 범위와 `primary_scope_anchor` 적용 기준은 `agent_rules/input/scope_anchor_rules.md`를 따른다.

```

  

### 3. P0 성격 유지

  

이 규칙은 P0 성격을 유지합니다. 분리 과정에서 P0 태그를 제거하지 않습니다.

  

## 완료 기준

  

- [ ] `agent_rules/input/scope_anchor_rules.md`가 생성되었다.

- [ ] 파일 조합별 본문 범위 규칙이 새 파일에 보존되었다.

- [ ] `primary_scope_anchor` 준수 원칙이 P0로 유지되었다.

- [ ] `input_guide.md`에는 참조 중심 문구만 남았다.

- [ ] 루트 `AGENTS.md`는 수정되지 않았다.

  

---

  

# Issue 9. PLAN 입력 판정 및 페이지 판독 기록을 input/input_plan_logging.md로 분리

  

## 문제

  

`input_guide.md`의 DOC_PLAN 설계 및 작성 섹션에는 입력 판정, 페이지 판독 기록, slide-only PLAN 기록 등이 들어 있습니다. 이는 입력 범위 규칙보다는 PLAN 기록 방식에 가까우며, 필요할 때만 읽히도록 분리하는 것이 좋습니다.

  

## 수정 대상

  

- `agent_rules/input_guide.md`

- `agent_rules/input/input_plan_logging.md`

  

## 요구 사항

  

### 1. 새 파일 생성

  

다음 섹션을 새 파일로 이동합니다.

  

```text

2. **DOC_PLAN 설계 및 작성 (Planning)**

```

  

새 파일 경로:

  

```text

agent_rules/input/input_plan_logging.md

```

  

포함할 내용:

  

- 본문 작성 전 PLAN에 입력 판정 기록

- PDF 판독 전략 기록 형식

- slide-only 입력 판정 기록

- Full PLAN의 페이지 판독 기록

- Mini PLAN에서 페이지 판독 기록 선택 사항

- Full PLAN에서 페이지 단위 또는 연속 구간 기록

- 파일별 판독 경로 기록

- 외부 자료 사용 목적 기록

  

### 2. `input_guide.md`에는 참조 문구만 남김

  

원래 위치에는 다음처럼 축약합니다.

  

```md

2. **DOC_PLAN 설계 및 작성 (Planning)**

   - 입력 판정, 판독 경로, 페이지 판독 기록 방식은 `agent_rules/input/input_plan_logging.md`를 따른다.

```

  

### 3. 렌더링 캐시 충돌은 별도 Issue로 남김

  

이 Issue에서는 기존 문구를 이동만 하고 의미를 바꾸지 않습니다.  

만약 `input_guide.md`에 렌더링 캐시 문구가 남아 있으면 그대로 이동합니다.

  

## 완료 기준

  

- [ ] `agent_rules/input/input_plan_logging.md`가 생성되었다.

- [ ] 입력 판정 및 페이지 판독 기록 규칙이 새 파일에 보존되었다.

- [ ] `input_guide.md`에는 참조 문구만 남았다.

- [ ] Mini/Full PLAN의 페이지 판독 기록 차이가 유지되었다.

- [ ] 루트 `AGENTS.md`는 수정되지 않았다.

  

---

  

# Issue 10. 이미지 트러블슈팅 규칙을 input/image_troubleshooting.md로 분리

  

## 문제

  

`input_guide.md`의 이미지 파일 트러블슈팅 규칙은 `.jpg`, `.png` 캡처본으로 blocker를 해결할 때만 필요합니다. 일반 강의노트 생성 작업에서는 매번 읽을 필요가 없습니다.

  

## 수정 대상

  

- `agent_rules/input_guide.md`

- `agent_rules/input/image_troubleshooting.md`

  

## 요구 사항

  

### 1. 새 파일 생성

  

다음 섹션을 새 파일로 이동합니다.

  

```text

3. **이미지 파일(`.jpg`, `.png`) 트러블슈팅 처리 규칙 (Resolving Blockers)**

```

  

새 파일 경로:

  

```text

agent_rules/input/image_troubleshooting.md

```

  

포함할 내용:

  

- PLAN의 트러블슈팅/이슈 섹션 확인

- 이미지 캡처본 판독

- 판독 불가 시 재요청 기본 2회

- 핵심 전제인 경우 추가 1회 요청 가능

- 총 재요청 3회 초과 금지

- 해결/실패/다음 단계 체크박스 예시

  

### 2. `input_guide.md`에는 참조 문구만 남김

  

원래 위치에는 다음처럼 축약합니다.

  

```md

3. **이미지 파일(`.jpg`, `.png`) 트러블슈팅 처리 규칙 (Resolving Blockers)**

   - 이미지 캡처본 기반 blocker 해결 규칙은 `agent_rules/input/image_troubleshooting.md`를 따른다.

```

  

### 3. 재요청 정책과 blocker 정책의 의미 유지

  

`plan/blocker_and_resume.md`와 의미가 충돌하지 않도록 원문 정책을 유지합니다.  

이 Issue에서는 다른 파일의 재요청 정책을 수정하지 않습니다.

  

## 완료 기준

  

- [ ] `agent_rules/input/image_troubleshooting.md`가 생성되었다.

- [ ] 이미지 트러블슈팅 규칙이 새 파일에 보존되었다.

- [ ] `input_guide.md`에는 참조 문구만 남았다.

- [ ] 재요청 제한이 기존 정책과 동일하게 유지되었다.

- [ ] 루트 `AGENTS.md`는 수정되지 않았다.

  

---

  

# Issue 11. note_creation_guide를 writing/ 하위 파일로 분리

  

## 문제

  

`note_creation_guide.md`에는 출력 언어 정책, 파일명/저장 경로, 본문 생성 원칙, 문서 구조, LaTeX 규칙, 예시가 함께 들어 있습니다. 실제 작업에서는 언어 정책이나 LaTeX 규칙만 필요한 경우도 있으므로 writing 하위 파일로 분리하는 것이 좋습니다.

  

## 수정 대상

  

- `agent_rules/note_creation_guide.md`

- `agent_rules/writing/output_language_policy.md`

- `agent_rules/writing/markdown_note_style.md`

- `agent_rules/writing/latex_style.md`

- `agent_rules/examples/note_examples.md`

  

## 요구 사항

  

### 1. 출력 언어 정책 분리

  

다음 섹션을 이동합니다.

  

```text

## [P1] Output Language Policy

```

  

새 파일:

  

```text

agent_rules/writing/output_language_policy.md

```

  

### 2. 문서 작성 및 서식 규칙 분리

  

파일명/저장 경로, 본문 생성 규칙, 문서 구조 및 서식 규칙 중 Markdown 작성 방식과 관련된 내용을 이동합니다.

  

새 파일:

  

```text

agent_rules/writing/markdown_note_style.md

```

  

포함할 내용:

  

- 파일명 규칙

- 저장 경로 규칙

- PLAN 파일 저장 경로

- 세부 내용 보존 원칙

- 긴 문서 자율 연속 작성

- 문서 구조

- H1/H2/H3 작성 방식

- 콘텐츠 유형별 포맷 분리

- 어투

  

### 3. LaTeX 규칙 분리

  

수식 작성 규칙을 이동합니다.

  

새 파일:

  

```text

agent_rules/writing/latex_style.md

```

  

포함할 내용:

  

- 인라인 수식

- 블록 수식

- GitHub Flavored LaTeX

- 블록 수식 앞뒤 줄바꿈

- 렌더링 안전성

  

### 4. 예시 분리

  

다음 예시 영역을 이동합니다.

  

```text

--- (마크다운 문서 내부 예시 포맷: 고체역학 3-1.md) ---

```

  

새 파일:

  

```text

agent_rules/examples/note_examples.md

```

  

### 5. 기존 파일은 인덱스로 축약

  
`note_creation_guide.md`는 다음 역할만 남깁니다.

  
- 이 가이드는 writing 하위 파일의 인덱스임을 명시

- 각 분리 파일의 용도와 읽는 시점 요약

- 기존 P0/P1 성격 유지

다음 파일 참조를 명시한다.

- `agent_rules/writing/output_language_policy.md`
- `agent_rules/writing/markdown_note_style.md`
- `agent_rules/writing/latex_style.md`
- `agent_rules/examples/note_examples.md`
  
## 추가 지시

- 이 Issue는 단일 Issue로 처리한다.
- 먼저 `note_creation_guide.md`에서 지정 앵커를 검색해 현재 섹션 구조를 확인한다.
- 기존 긴 본문을 Python 문자열 리터럴이나 WriteFile content로 다시 하드코딩하지 않는다.
- 원본 파일의 섹션 범위를 기준으로 이동하되, `## [P1] 3. 문서 구조 및 서식 규칙` 내부는 Markdown 규칙과 LaTeX 규칙을 의미 기준으로 나누어 이동한다.
- `latex_style.md`에는 수식 작성 규칙만 포함한다.
- `markdown_note_style.md`에는 파일명, 저장 경로, PLAN 경로, 본문 생성 원칙, 문서 구조, H1/H2/H3, 콘텐츠 유형별 포맷, 어투를 포함한다.
- 예시 블록은 `--- (마크다운 문서 내부 예시 포맷: 고체역학 3-1.md) ---`부터 EOF까지 `examples/note_examples.md`로 이동한다.
- `note_creation_guide.md`는 전체 재작성하지 말고 인덱스/요약/참조 중심으로 축약한다.
- 루트 `AGENTS.md`는 수정하지 않는다.
- 수정 후 `\x07`, `�`, 비허용 제어 문자가 0개인지 확인한다.
- 섹션 시작/종료 앵커가 불명확하거나, `## [P1] 3. 문서 구조 및 서식 규칙` 내부 분리가 불명확하면 임의 분리하지 말고 중단 보고한다.

## 완료 기준

  

- [ ] `writing/output_language_policy.md`가 생성되었다.

- [ ] `writing/markdown_note_style.md`가 생성되었다.

- [ ] `writing/latex_style.md`가 생성되었다.

- [ ] `examples/note_examples.md`가 생성되었다.

- [ ] `note_creation_guide.md`는 인덱스 역할로 축약되었다.

- [ ] 기존 Markdown 작성 규칙의 의미가 변경되지 않았다.

- [ ] 루트 `AGENTS.md`는 수정되지 않았다.

  

---

  

# Issue 12. validation_guide를 validation/ 하위 파일로 분리

  

## 문제

  

`validation_guide.md`에는 Required Checks, hard/soft rule, 입력 커버리지, 태그 분류, 리포트 기준이 모두 들어 있습니다. 검증 단계에서도 작업에 따라 구조 검증만 필요한 경우, 입력 커버리지까지 필요한 경우, 리포트 기준만 필요한 경우가 다르므로 분리하는 것이 좋습니다.

  

## 수정 대상

  

- `agent_rules/validation_guide.md`

- `agent_rules/validation/validation_overview.md`

- `agent_rules/validation/structure_and_latex_validation.md`

- `agent_rules/validation/input_coverage_validation.md`

- `agent_rules/validation/report_validation.md`

  

## 요구 사항

  

### 1. validation_overview.md 생성

  

다음 내용을 포함합니다.

  

- 검증 목적

- 필수 입력

- JSON canonical source 참조

- `schemas/validation_rules.json` 우선 원칙

- 검증 순서 요약

  

### 2. structure_and_latex_validation.md 생성

  

다음 내용을 이동합니다.

  

- Required Checks 중 구조 검증 관련 항목

- 경로 및 인코딩 규칙 참조

- LaTeX 검증 항목

- allowed/discouraged macro 설명

- Hard Rule / Soft Rule 중 제목, H1, H2 구분선, recommended_pattern 관련 설명

  

### 3. input_coverage_validation.md 생성

  

다음 내용을 이동합니다.

  

- 입력 커버리지 검증

- Mini PLAN / Full PLAN 입력 커버리지 차이

- slide-only 전체 슬라이드 검토 여부

- 페이지 판독 기록과 최종 본문 대조

- `primary_scope_anchor` 범위 충돌 확인

- Full PLAN에서 구조 검증과 입력 커버리지 검증 분리

  

### 4. report_validation.md 생성

  

다음 내용을 이동합니다.

  

- 태그 분류 기준

- 정상 판정/범위 관리 태그와 잔여 이슈 태그 처리

- 환경 이슈 기록

- hard/soft 결과 보고

- 최종 리포트 템플릿 선택

- `validation_report` 섹션 순서

- 변경 파일 상태 구분

  

### 5. 기존 파일은 인덱스로 축약

  

`validation_guide.md`는 다음만 남깁니다.

  

- validation 하위 파일 목록

- 각 파일을 읽는 시점

- `schemas/validation_rules.json`과 `schemas/report_templates.json`을 우선한다는 원칙

  

## 완료 기준

  

- [ ] `validation/validation_overview.md`가 생성되었다.

- [ ] `validation/structure_and_latex_validation.md`가 생성되었다.

- [ ] `validation/input_coverage_validation.md`가 생성되었다.

- [ ] `validation/report_validation.md`가 생성되었다.

- [ ] `validation_guide.md`는 인덱스 역할로 축약되었다.

- [ ] hard/soft rule 의미가 유지되었다.

- [ ] 입력 커버리지 검증 의미가 유지되었다.

- [ ] 루트 `AGENTS.md`는 수정되지 않았다.

  

---

  

# Issue 13. codex_execution_guide를 execution/ 하위 파일로 분리

  

## 문제

  

`codex_execution_guide.md`에는 workspace 확인, git 상태, 기존 변경 보호, JSON 누락, JSON canonical source, 위험 명령, 인코딩, Python, PDF 렌더링, 검증 실행, 실패 처리, 작업 후 보고가 모두 들어 있습니다. 파일 변경이 없는 작업에서도 전체 실행 가이드를 읽게 되면 부담이 큽니다.

  

## 수정 대상

  

- `agent_rules/codex_execution_guide.md`

- `agent_rules/execution/git_and_file_changes.md`

- `agent_rules/execution/command_safety.md`

- `agent_rules/execution/python_and_pdf_rendering.md`

- `agent_rules/execution/json_and_reporting.md`

  

## 요구 사항

  

### 1. git_and_file_changes.md 생성

  

다음 내용을 이동합니다.

  

- Workspace 확인

- Git 상태 확인

- 기존 변경 보호

- 파일 변경 원칙

- 작업 후 변경 내역 확인

- modified/deleted/untracked/staged 구분

- 작업 전 dirty 상태와 이번 변경 구분

  

### 2. command_safety.md 생성

  

다음 내용을 이동합니다.

  

- Windows 호환성 및 UTF-8 인코딩 강제

- PowerShell profile 오류 분리

- PowerShell 스크립트 실행 기준

- 위험 명령 제한

- 설치 금지 기본값

- 네트워크 접근 원칙

- `schemas/codex_policy.json` 참조

  

### 3. python_and_pdf_rendering.md 생성

  

다음 내용을 이동합니다.

  

- Python 실행 환경 고정

- `conda` → `python` → `py` fallback

- `py --version` 실패 처리

- PDF 페이지 렌더링 방식 우선순위

- Edge screenshot 최후 수단

- 렌더링 실패 기록

- 렌더링 이미지 캐시 보관 모드

  

### 4. json_and_reporting.md 생성

  

다음 내용을 이동합니다.

  

- JSON 누락 시 처리

- JSON Canonical Source 원칙

- 가이드 재확인 기준

- 검증 실행 절차

- 실패 처리 정책

- 작업 요약 및 최종 보고 형식

- `schemas/report_templates.json` 참조

  

### 5. 기존 파일은 인덱스로 축약

  

`codex_execution_guide.md`는 다음 역할만 남깁니다.

  

- execution 하위 파일 목록

- 각 파일을 읽는 시점

- P0 안전 정책은 분리 후에도 유지된다는 문구

- `schemas/codex_policy.json`과 `schemas/report_templates.json` 우선 원칙

  

## 완료 기준

  

- [ ] `execution/git_and_file_changes.md`가 생성되었다.

- [ ] `execution/command_safety.md`가 생성되었다.

- [ ] `execution/python_and_pdf_rendering.md`가 생성되었다.

- [ ] `execution/json_and_reporting.md`가 생성되었다.

- [ ] `codex_execution_guide.md`는 인덱스 역할로 축약되었다.

- [ ] 위험 명령 제한, 기존 변경 보호, UTF-8 인코딩, git 상태 확인이 약화되지 않았다.

- [ ] 렌더링 캐시 보관 정책이 유지되었다.

- [ ] 루트 `AGENTS.md`는 수정되지 않았다.

  

---

# Issue 14. 기존 대형 가이드의 중복 제거 및 내부 참조 정리 — 분할 처리

## 공통 선행 조건

다음 Issue가 모두 완료된 뒤 진행한다.

- Issue 2
- Issue 3
- Issue 4
- Issue 5
- Issue 6
- Issue 7
- Issue 8
- Issue 9
- Issue 10
- Issue 11
- Issue 12
- Issue 13

## 공통 지시

- Issue 14 전체를 한 번에 처리하지 않는다.
- 아래 하위 Issue 중 **하나만 선택하여 처리**한다.
- 루트 `AGENTS.md`는 수정하지 않는다.
- 기존 대형 가이드는 인덱스/요약/하위 파일 참조 중심으로 축약한다.
- 이미 새 파일로 이동한 긴 규칙 본문은 기존 파일에서 제거한다.
- 단, P0 안전 정책 요약, 읽는 시점 안내, JSON canonical source 요약, `primary_scope_anchor` 같은 핵심 원칙의 한 줄 요약은 유지할 수 있다.
- 중복 제거 과정에서 기존 P0 안전 정책을 삭제하거나 약화하지 않는다.
- 작업 후 `git status --short`와 변경 파일 요약을 보고한다.

---

# Issue 14-A. `memory_management_guideline.md` 인덱스 정리

## 문제

앞선 Issue에서 PLAN 예시, PLAN 언어 정책, Mini/Full PLAN 정책, blocker/재개 정책이 별도 파일로 분리되면 `memory_management_guideline.md`에 중복 규칙이 남을 수 있습니다.

## 수정 대상

- `agent_rules/memory_management_guideline.md`

## 요구 사항

### 1. 인덱스 역할로 축약

`memory_management_guideline.md`를 다음 역할 중심으로 정리합니다.

- PLAN 관리 가이드의 상위 인덱스
- 각 하위 파일을 읽는 시점 안내
- PLAN 관리의 핵심 원칙 요약
- Mini/Full PLAN, blocker, 언어 정책, 예시 파일 참조

### 2. 하위 파일 참조 정리

다음 파일 참조를 명시합니다.

```text
agent_rules/plan/plan_language_policy.md
agent_rules/plan/mini_full_plan.md
agent_rules/plan/blocker_and_resume.md
agent_rules/examples/plan_examples.md
````

### 3. 중복 본문 제거

이미 하위 파일로 이동한 긴 본문은 제거합니다.

- PLAN 언어 정책 상세 본문
    
- Mini/Full PLAN 상세 기준
    
- blocker/재요청/재개 상세 규칙
    
- 긴 PLAN 예시
    

### 4. 유지할 수 있는 내용

다음은 짧게 유지할 수 있습니다.

- PLAN은 차시 단위로 관리한다는 원칙
    
- 불확실성은 본문에 무리하게 병합하지 않고 PLAN/최종 보고서에 기록한다는 원칙
    
- 실제 문서 작성 작업에서만 PLAN을 갱신한다는 원칙
    

## 완료 기준

-  `memory_management_guideline.md`가 인덱스/요약 중심으로 축약되었다.
    
-  PLAN 관련 하위 파일 참조가 명확하다.
    
-  새 파일로 이동한 긴 규칙 본문이 중복으로 남아 있지 않다.
    
-  PLAN 관리 핵심 원칙은 유지되었다.
    
-  루트 `AGENTS.md`는 수정되지 않았다.
    
-  작업 후 `git status --short`와 변경 파일 요약이 보고되었다.
    

---

# Issue 14-B. `input_guide.md` 인덱스 정리

## 문제

앞선 Issue에서 PDF 판독 전략, 불확실성 태그, `primary_scope_anchor` 범위 규칙, PLAN 입력 기록, 이미지 트러블슈팅 규칙이 별도 파일로 분리되면 `input_guide.md`에 중복 규칙이 남을 수 있습니다.

## 수정 대상

- `agent_rules/input_guide.md`
    

## 요구 사항

### 1. 인덱스 역할로 축약

`input_guide.md`를 다음 역할 중심으로 정리합니다.

- 입력 처리 가이드의 상위 인덱스
    
- 각 하위 파일을 읽는 시점 안내
    
- `primary_scope_anchor` 핵심 원칙 요약
    
- 입력 조합별 범위 규칙, PDF 판독, 태그, PLAN 기록, 이미지 트러블슈팅 파일 참조
    

### 2. 하위 파일 참조 정리

다음 파일 참조를 명시합니다.

```text
agent_rules/input/scope_anchor_rules.md
agent_rules/input/pdf_reading_strategy.md
agent_rules/input/uncertainty_tags.md
agent_rules/input/input_plan_logging.md
agent_rules/input/image_troubleshooting.md
```

### 3. 중복 본문 제거

이미 하위 파일로 이동한 긴 본문은 제거합니다.

- 파일 조합별 본문 범위 상세 규칙
    
- slide-only 상세 규칙
    
- PDF 텍스트 추출 fallback 상세 규칙
    
- 정상 판정/잔여 이슈 태그 상세 목록
    
- 페이지 판독 기록 상세 형식
    
- 이미지 트러블슈팅 상세 절차
    

### 4. 유지할 수 있는 내용

다음은 짧게 유지할 수 있습니다.

- 본문 범위는 `primary_scope_anchor`가 결정한다.
    
- 보조 자료와 외부 자료는 명시적으로 허용된 경우를 제외하고 본문 범위를 확장하지 않는다.
    
- 판독 불확실성은 본문에 단정적으로 병합하지 않고 이슈로 기록한다.
    
- PDF 판독 전략과 태그 정의는 하위 파일을 따른다.
    

## 완료 기준

-  `input_guide.md`가 인덱스/요약 중심으로 축약되었다.
    
-  입력 관련 하위 파일 참조가 명확하다.
    
-  새 파일로 이동한 긴 규칙 본문이 중복으로 남아 있지 않다.
    
-  `primary_scope_anchor` 핵심 원칙이 유지되었다.
    
-  외부 자료/보조 자료의 본문 범위 확장 금지 원칙이 유지되었다.
    
-  루트 `AGENTS.md`는 수정되지 않았다.
    
-  작업 후 `git status --short`와 변경 파일 요약이 보고되었다.
    

---

# Issue 14-C. `note_creation_guide.md` 인덱스 정리

## 문제

앞선 Issue에서 출력 언어 정책, Markdown 작성 스타일, LaTeX 스타일, 노트 예시가 별도 파일로 분리되면 `note_creation_guide.md`에 중복 규칙이 남을 수 있습니다.

## 수정 대상

- `agent_rules/note_creation_guide.md`
    

## 요구 사항

### 1. 인덱스 역할로 축약

`note_creation_guide.md`를 다음 역할 중심으로 정리합니다.

- Markdown 노트 작성 가이드의 상위 인덱스
    
- 각 하위 파일을 읽는 시점 안내
    
- 최종 강의노트 작성의 핵심 원칙 요약
    
- 출력 언어, Markdown 스타일, LaTeX 스타일, 예시 파일 참조
    

### 2. 하위 파일 참조 정리

다음 파일 참조를 명시합니다.

```text
agent_rules/writing/output_language_policy.md
agent_rules/writing/markdown_note_style.md
agent_rules/writing/latex_style.md
agent_rules/examples/note_examples.md
```

### 3. 중복 본문 제거

이미 하위 파일로 이동한 긴 본문은 제거합니다.

- 출력 언어 정책 상세 본문
    
- 파일명/저장 경로 상세 규칙
    
- 문서 구조 및 서식 상세 규칙
    
- LaTeX 작성 상세 규칙
    
- 긴 Markdown 노트 예시
    

### 4. 유지할 수 있는 내용

다음은 짧게 유지할 수 있습니다.

- 최종 강의노트는 한국어가 기본이다.
    
- 사용자가 명시적으로 요청한 경우에만 영어 노트를 작성한다.
    
- 세부 내용, 예시, 도출 과정은 요약 요청이 없는 한 생략하지 않는다.
    
- 구체적 작성 규칙은 `writing/` 하위 파일을 따른다.
    

## 완료 기준

-  `note_creation_guide.md`가 인덱스/요약 중심으로 축약되었다.
    
-  writing 관련 하위 파일 참조가 명확하다.
    
-  새 파일로 이동한 긴 규칙 본문이 중복으로 남아 있지 않다.
    
-  최종 강의노트 한국어 기본 원칙이 유지되었다.
    
-  세부 내용 보존 원칙이 유지되었다.
    
-  루트 `AGENTS.md`는 수정되지 않았다.
    
-  작업 후 `git status --short`와 변경 파일 요약이 보고되었다.
    

---

# Issue 14-D. `validation_guide.md` 인덱스 정리

## 문제

앞선 Issue에서 validation overview, 구조/LaTeX 검증, 입력 커버리지 검증, 리포트 검증 기준이 별도 파일로 분리되면 `validation_guide.md`에 중복 규칙이 남을 수 있습니다.

## 수정 대상

- `agent_rules/validation_guide.md`
    

## 요구 사항

### 1. 인덱스 역할로 축약

`validation_guide.md`를 다음 역할 중심으로 정리합니다.

- 검증 가이드의 상위 인덱스
    
- 각 하위 파일을 읽는 시점 안내
    
- JSON canonical source 원칙 요약
    
- `validation_rules.json`과 `report_templates.json` 우선 원칙
    
- 구조/LaTeX 검증, 입력 커버리지 검증, 리포트 기준 파일 참조
    

### 2. 하위 파일 참조 정리

다음 파일 참조를 명시합니다.

```text
agent_rules/validation/validation_overview.md
agent_rules/validation/structure_and_latex_validation.md
agent_rules/validation/input_coverage_validation.md
agent_rules/validation/report_validation.md
agent_rules/schemas/validation_rules.json
agent_rules/schemas/report_templates.json
```

### 3. 중복 본문 제거

이미 하위 파일로 이동한 긴 본문은 제거합니다.

- Required Checks 상세 목록
    
- Hard Rule / Soft Rule 상세 설명
    
- 입력 커버리지 검증 상세 규칙
    
- 태그 분류 상세 기준
    
- 환경 이슈/리포트 기준 상세 설명
    
- validation_report 섹션 순서 상세 설명
    

### 4. 유지할 수 있는 내용

다음은 짧게 유지할 수 있습니다.

- 검증은 작성 완료 후, PLAN 업데이트 후, 최종 리포트 출력 전 적용한다.
    
- 기계적 검증 규칙은 `schemas/validation_rules.json`을 따른다.
    
- 최종 리포트 템플릿은 `schemas/report_templates.json`을 따른다.
    
- hard rule 위반과 soft rule 위반은 구분한다.
    

## 완료 기준

-  `validation_guide.md`가 인덱스/요약 중심으로 축약되었다.
    
-  validation 하위 파일 및 schema 파일 참조가 명확하다.
    
-  새 파일로 이동한 긴 규칙 본문이 중복으로 남아 있지 않다.
    
-  JSON canonical source 원칙이 유지되었다.
    
-  hard/soft rule 구분 원칙이 유지되었다.
    
-  루트 `AGENTS.md`는 수정되지 않았다.
    
-  작업 후 `git status --short`와 변경 파일 요약이 보고되었다.
    

---

# Issue 14-E. `codex_execution_guide.md` 인덱스 정리

## 문제

앞선 Issue에서 git/file 변경, command safety, Python/PDF rendering, JSON/reporting 규칙이 별도 파일로 분리되면 `codex_execution_guide.md`에 중복 규칙이 남을 수 있습니다.

## 수정 대상

- `agent_rules/codex_execution_guide.md`
    

## 요구 사항

### 1. 인덱스 역할로 축약

`codex_execution_guide.md`를 다음 역할 중심으로 정리합니다.

- Codex 실행 가이드의 상위 인덱스
    
- 각 하위 파일을 읽는 시점 안내
    
- P0 안전 정책 요약
    
- JSON canonical source 원칙 요약
    
- execution 하위 파일과 schema 파일 참조
    

### 2. 하위 파일 참조 정리

다음 파일 참조를 명시합니다.

```text
agent_rules/execution/git_and_file_changes.md
agent_rules/execution/command_safety.md
agent_rules/execution/python_and_pdf_rendering.md
agent_rules/execution/json_and_reporting.md
agent_rules/schemas/codex_policy.json
agent_rules/schemas/report_templates.json
```

### 3. 중복 본문 제거

이미 하위 파일로 이동한 긴 본문은 제거합니다.

- workspace/git 상태 상세 절차
    
- 기존 변경 보호 상세 규칙
    
- PowerShell/UTF-8 상세 규칙
    
- 위험 명령 상세 규칙
    
- Python fallback 상세 규칙
    
- PDF 렌더링 상세 규칙
    
- JSON 누락 및 보고 형식 상세 규칙
    
- 작업 후 diff 확인 상세 절차
    

### 4. 유지할 수 있는 내용

다음은 짧게 유지할 수 있습니다.

- 파일 변경 전후에는 execution 하위 가이드를 따른다.
    
- 위험 명령 제한, 기존 변경 보호, UTF-8 인코딩, git 상태 확인은 P0 안전 정책이다.
    
- 위험 명령과 git check의 기계적 기준은 `schemas/codex_policy.json`을 따른다.
    
- 최종 보고 템플릿은 `schemas/report_templates.json`을 따른다.
    

## 완료 기준

-  `codex_execution_guide.md`가 인덱스/요약 중심으로 축약되었다.
    
-  execution 하위 파일 및 schema 파일 참조가 명확하다.
    
-  새 파일로 이동한 긴 규칙 본문이 중복으로 남아 있지 않다.
    
-  위험 명령 제한, 기존 변경 보호, UTF-8 인코딩, git 상태 확인 원칙이 유지되었다.
    
-  JSON canonical source 원칙이 유지되었다.
    
-  루트 `AGENTS.md`는 수정되지 않았다.
    
-  작업 후 `git status --short`와 변경 파일 요약이 보고되었다.
    

---

# Issue 14-F. 내부 참조 최종 검색 검증

## 문제

Issue 14-A~14-E로 기존 대형 가이드를 인덱스화한 뒤에도, 오래된 파일 경로 참조나 중복 본문이 일부 남아 있을 수 있습니다. 마지막으로 새 구조의 내부 참조를 검색 기반으로 확인해야 합니다.

## 선행 조건

다음 Issue가 모두 완료된 뒤 진행한다.

- Issue 14-A
    
- Issue 14-B
    
- Issue 14-C
    
- Issue 14-D
    
- Issue 14-E
    

## 수정 대상

- `agent_rules/` 내부 Markdown 파일 전체 중 검색 결과로 확인된 파일
    
- 루트 `AGENTS.md`는 수정하지 않음
    

## 요구 사항

### 1. 새 하위 파일 참조 확인

다음 경로들이 필요한 인덱스 파일에서 참조되는지 검색합니다.

```text
agent_rules/plan/plan_language_policy.md
agent_rules/plan/mini_full_plan.md
agent_rules/plan/blocker_and_resume.md
agent_rules/examples/plan_examples.md
agent_rules/input/scope_anchor_rules.md
agent_rules/input/pdf_reading_strategy.md
agent_rules/input/uncertainty_tags.md
agent_rules/input/input_plan_logging.md
agent_rules/input/image_troubleshooting.md
agent_rules/writing/output_language_policy.md
agent_rules/writing/markdown_note_style.md
agent_rules/writing/latex_style.md
agent_rules/examples/note_examples.md
agent_rules/validation/validation_overview.md
agent_rules/validation/structure_and_latex_validation.md
agent_rules/validation/input_coverage_validation.md
agent_rules/validation/report_validation.md
agent_rules/execution/git_and_file_changes.md
agent_rules/execution/command_safety.md
agent_rules/execution/python_and_pdf_rendering.md
agent_rules/execution/json_and_reporting.md
```

### 2. 오래된 중복 본문 검색

다음 대표 키워드를 검색하여 기존 대형 파일에 긴 본문이 중복으로 남아 있는지 확인합니다.

```text
PLAN files may mix English and Korean
Mini PLAN
Full PLAN
blocked 전환 조건
충돌 및 불확실성 태그
PDF 판독 전략
primary_scope_anchor
DOC_PLAN 설계 및 작성
이미지 파일
Output Language Policy
GitHub Flavored LaTeX
Required Checks
Hard Rule / Soft Rule
입력 커버리지 검증
위험 명령 제한
Python 실행 환경 고정
PDF 페이지 렌더링 방식 우선순위
JSON Canonical Source
```

### 3. 오래된 참조 수정

검색 결과 다음에 해당하는 경우만 수정합니다.

- 새 구조에서 더 이상 권장되지 않는 옛 참조
    
- 하위 파일로 옮겨진 긴 본문이 기존 대형 파일에 그대로 남은 경우
    
- 명백히 잘못된 경로
    
- 동일한 규칙이 두 곳에서 서로 다르게 설명되는 경우
    

### 4. 수정하지 않을 경우

검색 결과가 단순 요약 문구이거나 의도적으로 남긴 핵심 원칙이면 수정하지 않습니다. 최종 보고에 “요약 문구로 유지”라고 기록합니다.

## 완료 기준

-  새 하위 파일 경로가 필요한 인덱스 파일에서 참조된다.
    
-  오래된 경로 참조가 발견되면 수정되었다.
    
-  기존 대형 파일에 긴 중복 본문이 남아 있으면 제거 또는 참조 문구로 축약되었다.
    
-  의도적으로 유지한 핵심 요약 문구는 최종 보고에 구분되었다.
    
-  루트 `AGENTS.md`는 수정되지 않았다.
    
-  작업 후 `git status --short`와 변경 파일 요약이 보고되었다.