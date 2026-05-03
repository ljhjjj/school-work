# agent_rules 리팩토링 Issue 요청서 — 수정본

> 목적: `agent_rules/`의 복잡도, 중복 규칙, 검증 경직성, PLAN 관리 부담을 줄이되 기존 P0 안전 정책은 약화하지 않는다.  
> 권장 사용 방식: **한 번에 하나의 Issue만** 붙여서 처리한다.  
> 권장 처리 순서: Issue 1 → 2 → 3 → 4 → 5 → 6 → 7 → 8-A → 8-B → 8-C → 8-D

---

# 공통 지시

```text
수정 전 관련 파일을 먼저 읽고 현재 섹션 구조를 확인할 것.
기존 규칙의 표현 방식과 문체를 최대한 유지할 것.
한 Issue만 처리하고, 다른 Issue의 내용은 불필요하게 섞지 말 것.
Issue 요구사항과 충돌하지 않는 기존 P0 안전 정책은 절대 삭제하거나 약화하지 말 것.
기존 문구를 제거하는 경우, 제거 사유와 대체 위치를 작업 보고에 명시할 것.
JSON 파일을 수정한 경우 PowerShell ConvertFrom-Json 또는 동등한 JSON 파서로 검증할 것.
작업 후 git status --short와 변경 파일 요약을 보고할 것.
수정 사항이 기존 안전 정책(위험 명령 제한, 기존 변경 보호, UTF-8 인코딩)을 약화시키지 않도록 확인할 것.

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

# Issue 1. JSON 스키마를 canonical source로 정리

## 문제

동일한 규칙이 JSON 스키마와 Markdown 가이드에 중복/분산되어 있습니다.

- `dangerous_commands`: `codex_execution_guide.md`에도 서술되어 있고 `schemas/codex_policy.json`에도 존재함
- `report_templates`: `codex_execution_guide.md`에도 언급되어 있고 `schemas/report_templates.json`에도 존재함
- `validation` 체크항목: `validation_guide.md`에도 서술되어 있고 `schemas/validation_rules.json`에도 존재함

이 구조에서는 규칙 변경 시 여러 파일을 동시에 수정해야 하며, 누락 시 JSON과 Markdown이 서로 다른 규칙을 말하는 상태가 될 수 있습니다.

## 수정 대상

- `agent_rules/System Prompt Autonomous Markdown.md`
- `agent_rules/codex_execution_guide.md`
- `agent_rules/validation_guide.md`
- `agent_rules/input_acquisition_guide.md`
- `agent_rules/schemas/codex_policy.json`
- `agent_rules/schemas/validation_rules.json`
- `agent_rules/schemas/report_templates.json`

## 요구 사항

### 1. JSON canonical source 원칙 명시

`System Prompt` 또는 `codex_execution_guide.md`에 다음 원칙을 추가합니다.

```md
## JSON Canonical Source 원칙

- `schemas/` 폴더의 JSON 파일은 해당 도메인의 유일한 기계 해석 가능한 규칙(canonical source)이다.
- Markdown 가이드는 규칙의 적용 맥락, 예외 상황, 사용 절차를 설명한다.
- JSON과 Markdown이 충돌할 경우 JSON을 우선한다.
- 단, 위험 명령 제한, 기존 변경 보호, UTF-8 인코딩 등 P0 안전 정책은 JSON 누락 여부와 무관하게 항상 유지한다.
```

### 2. `codex_execution_guide.md`의 위험 명령 중복 서술 제거

위험 명령 제한 섹션을 JSON 참조 중심으로 바꿉니다.

```md
### 위험 명령 제한

- 위험 명령 분류 및 정책은 `schemas/codex_policy.json`의 `dangerous_commands`를 따른다.
- 조회 전용 명령과 변경 명령의 구분은 해당 JSON의 `read_only_commands`와 `requires_explicit_user_approval`을 따른다.
- Markdown 본문에는 카테고리별 세부 패턴을 반복 서술하지 않는다.
- JSON 누락 시에도 삭제, 초기화, 권한 우회, 외부 코드 실행 등 명백한 고위험 명령은 사용자 승인 없이 실행하지 않는다.
```

기존의 카테고리별 상세 패턴 나열은 제거하거나 JSON 참조로 축약합니다.

### 3. `validation_guide.md`의 Required Checks 중복 축약

`validation_guide.md`의 Required Checks를 다음처럼 JSON 참조 중심으로 정리합니다.

```md
## Required Checks

- 구조 검증 항목은 `schemas/validation_rules.json`의 `markdown_structure`, `latex`, `input_coverage`를 따른다.
- 파일 경로 및 인코딩 규칙은 해당 JSON의 `paths`, `encoding`을 따른다.
- 태그 분류 기준은 해당 JSON의 `normal_status_tags`와 `residual_issue_tags`를 따른다.
- Markdown 가이드는 검증 절차와 보고 방식만 설명한다.
```

### 4. report template 참조 단순화

`codex_execution_guide.md`의 최종 보고 형식 관련 섹션을 다음처럼 정리합니다.

```md
### 작업 요약 및 최종 보고 형식

- 최종 리포트는 `schemas/report_templates.json`의 템플릿을 선택한다.
- 템플릿 선택 기준은 해당 JSON의 `template_selection`을 따른다.
- Markdown 본문에는 개별 템플릿 내용을 반복 서술하지 않는다.
```

### 5. JSON 누락 fallback 정책 보완

기존의 “기본 지식으로 대체 실행” 표현을 약화하고, 누락된 검증은 생략 후 명시 보고하도록 수정합니다.

```md
### JSON 누락 시 처리

- JSON 파일이 누락된 경우, 해당 JSON이 필요한 기계적 검증/분류는 수행하지 않는다.
- 단, 비파괴적 읽기, 문서 검토, 일반적인 Markdown 작성은 계속할 수 있다.
- 위험 명령 제한, 기존 변경 보호, UTF-8 인코딩 등 P0 안전 정책은 System Prompt의 중앙 P0 규칙을 따른다.
- 작업 완료 후 `[⚠️ 실행 이슈]`에 어떤 JSON 파일이 없어 어떤 검증 또는 분류를 생략했는지 기록한다.
- 누락된 JSON 규칙을 임의의 기억이나 추측으로 완전히 대체하지 않는다.
```

## 완료 기준

- [ ] JSON이 canonical source이며 충돌 시 JSON 우선이라는 원칙이 명시되어 있다.
- [ ] `codex_execution_guide.md`의 위험 명령 제한이 JSON 참조 중심으로 축약되었다.
- [ ] `validation_guide.md`의 Required Checks가 JSON 참조 중심으로 축약되었다.
- [ ] `codex_execution_guide.md`의 보고 형식 설명이 `report_templates.json` 참조 중심으로 축약되었다.
- [ ] JSON 누락 시 “기본 지식으로 강행”이 아니라 “해당 검증 생략 + 실행 이슈 보고”로 정리되었다.
- [ ] JSON 파싱 검증이 통과한다.

---

# Issue 2. validation_rules에 hard/soft severity 체계 도입

## 문제

`schemas/validation_rules.json`의 검증 항목이 실제 실패로 봐야 하는 규칙과 단순 권장 형식을 충분히 구분하지 못합니다. 그 결과 제목 형식, LaTeX 스타일, PLAN 상세도 같은 soft rule이 경로, 인코딩, H1 존재 여부 같은 hard rule과 같은 수준으로 취급될 수 있습니다.

## 수정 대상

- `agent_rules/schemas/validation_rules.json`
- `agent_rules/validation_guide.md`

## 요구 사항

### 1. 최상위 `severity` 필드 추가

`validation_rules.json`에 다음과 같은 최상위 필드를 추가합니다.

```json
{
  "severity": {
    "hard_rules": [
      "paths",
      "encoding",
      "h1_required_count",
      "h2_horizontal_rule",
      "forbidden_latex_patterns",
      "primary_scope_anchor_coverage"
    ],
    "soft_rules": [
      "h1_recommended_pattern",
      "h2_recommended_pattern",
      "h3_recommended_pattern",
      "latex_discouraged_macros",
      "plan_detail_level"
    ]
  }
}
```

현재 JSON 구조와 충돌하지 않도록 기존 필드와 병합합니다.

### 2. hard rule / soft rule 의미 정의

`validation_guide.md`에 다음 원칙을 추가합니다.

```md
## Hard Rule / Soft Rule

- Hard rule 위반은 작업 완료로 인정하지 않는다. 가능한 경우 자동 수정하고, 자동 수정이 불가능하면 최종 리포트의 잔여 이슈에 명시한다.
- Soft rule 위반은 작업을 중단하지 않는다. 최종 리포트 또는 PLAN의 `[⚠️ 실행 이슈]` / `잔여 이슈`에 기록한다.
- H1이 아예 없는 경우는 hard rule 위반이다.
- H1이 존재하지만 권장 제목 형식에 맞지 않는 경우는 soft rule 위반이다.
- H2 구분선 누락은 문서 구조 일관성을 위해 hard rule로 유지한다.
```

### 3. 기존 검증 절차에 severity 반영

`validation_guide.md`의 최종 검증 절차에 다음을 반영합니다.

```md
- 검증 결과는 hard rule 위반과 soft rule 위반으로 나누어 보고한다.
- hard rule 위반이 남아 있으면 `completed`로 처리하지 않는다.
- soft rule 위반만 남아 있으면 `completed_with_warnings` 또는 이에 준하는 상태로 보고할 수 있다.
```

## 완료 기준

- [ ] `validation_rules.json`에 `severity.hard_rules`와 `severity.soft_rules`가 추가되었다.
- [ ] H1 존재 여부와 H1 권장 패턴 위반이 구분된다.
- [ ] hard rule 위반과 soft rule 위반의 처리 방식이 `validation_guide.md`에 명시되어 있다.
- [ ] H2 구분선 누락은 hard rule로 유지된다.
- [ ] JSON 파싱 검증이 통과한다.

---

# Issue 3. Markdown 제목 검증 패턴 완화

## 문제

`schemas/validation_rules.json`의 `markdown_structure` 제목 패턴이 너무 엄격합니다.

- H1: `^# .+ \d+-\d+: .+$` → 반드시 “과목명 주차-차시: 주제” 형식만 허용
- H2: `^## \d+\. .+` → 반드시 숫자로 시작해야 함
- H3: `^### \d+\) .+` → 반드시 숫자 + `)`로 시작해야 함

이 구조는 특강, 세미나, 실습, 요약형 문서처럼 주차-차시 또는 번호 섹션이 어울리지 않는 문서에서 불필요한 검증 실패를 유발합니다. 단, `\d+`는 두 자리 주차도 허용하고 `.+`는 대부분의 특수문자를 허용하므로 자릿수 문제나 특수문자 문제로 설명하지 않습니다.

## 수정 대상

- `agent_rules/schemas/validation_rules.json`
- `agent_rules/validation_guide.md`

## 요구 사항

### 1. H1 패턴 완화

`validation_rules.json`의 H1 규칙을 다음처럼 수정합니다.

```json
"h1": {
  "required_count": 1,
  "pattern": "^# .+$",
  "recommended_pattern": "^# .+ \\d+-\\d+: .+$"
}
```

- `required_count: 1`은 hard rule로 유지합니다.
- `pattern: ^# .+$`는 임의의 H1 제목을 허용합니다.
- 기존 형식은 `recommended_pattern`으로 이동합니다.
- H1이 존재하지만 `recommended_pattern`과 맞지 않는 경우만 soft warning으로 처리합니다.

### 2. H2/H3 숫자 제목 완화

`validation_rules.json`의 H2/H3 규칙을 다음처럼 수정합니다.

```json
"h2": {
  "pattern": "^## .+",
  "recommended_pattern": "^## \\d+\\. .+",
  "require_horizontal_rule_at_section_end": true,
  "horizontal_rule_position": "before_next_h2_or_end_of_document",
  "horizontal_rule_must_not_immediately_follow_h2": true
},
"h3": {
  "pattern": "^### .+",
  "recommended_pattern": "^### \\d+\\) .+",
  "required": false
}
```

- H2/H3의 기본 패턴은 자유 제목을 허용합니다.
- 기존 숫자 제목 형식은 `recommended_pattern`으로 유지합니다.
- H2 구분선 규칙은 hard rule로 유지합니다.
- H3는 선택적 구조로 유지합니다.

### 3. validation guide 반영

`validation_guide.md`에 다음을 반영합니다.

```md
- 제목이 존재하지 않는 경우와 권장 제목 형식에 맞지 않는 경우를 구분한다.
- H1 required count 위반은 hard rule이다.
- H1/H2/H3가 recommended_pattern에 맞지 않는 경우는 soft rule이다.
- 자유 제목은 허용하되, 강의노트 표준 형식에서는 recommended_pattern을 권장한다.
```

## 완료 기준

- [ ] H1 기본 패턴이 `^# .+$`로 완화되었다.
- [ ] 기존 H1 형식은 `recommended_pattern`으로 이동되었다.
- [ ] H2/H3 기본 패턴이 자유 제목을 허용하도록 완화되었다.
- [ ] 기존 H2/H3 숫자 제목 형식은 `recommended_pattern`으로 이동되었다.
- [ ] H1 없음은 hard rule, 권장 형식 불일치는 soft rule로 구분된다.
- [ ] JSON 파싱 검증이 통과한다.

---

# Issue 4. LaTeX allowed/discouraged macro 정책 정리

## 문제

`schemas/validation_rules.json`의 `latex.discouraged_macros`에 공학/수학 강의노트에서 일반적으로 쓰이는 표준 매크로가 포함되어 있습니다.

- `\underbrace`
- `\overbrace`
- `\begin{array}`
- `\begin{aligned}`

현재는 `severity: warning`, `action: report_only`라서 생성을 막지는 않지만, 검증 리포트에 불필요한 warning이 늘어나 실제 중요한 이슈가 묻힐 수 있습니다.

## 수정 대상

- `agent_rules/schemas/validation_rules.json`
- `agent_rules/validation_guide.md`

## 요구 사항

### 1. 표준 매크로 허용 목록 추가

`validation_rules.json`의 `latex` 섹션에 `allowed_macros`를 추가합니다.

```json
"allowed_macros": [
  {
    "pattern": "\\\\begin\\{aligned\\}",
    "note": "standard multi-line equation environment"
  },
  {
    "pattern": "\\\\begin\\{array\\}",
    "note": "standard tabular math environment"
  },
  {
    "pattern": "\\\\underbrace",
    "note": "standard annotation macro"
  },
  {
    "pattern": "\\\\overbrace",
    "note": "standard annotation macro"
  }
]
```

### 2. discouraged_macros 재정리

`aligned`, `array`, `underbrace`, `overbrace`는 `discouraged_macros`에서 제거합니다.

`discouraged_macros`에는 실제로 deprecated되었거나 렌더링 문제가 잦은 매크로만 남깁니다.

예시:

```json
"discouraged_macros": [
  {
    "pattern": "\\\\begin\\{eqnarray\\}",
    "severity": "warning",
    "action": "report_only",
    "reason": "deprecated; use aligned instead"
  }
]
```

### 3. validation guide 반영

`validation_guide.md`의 LaTeX 검증 항목을 다음처럼 수정합니다.

```md
- `latex` 규칙에 따라 인라인/블록 수식, 금지 패턴(`<math>` 태그 등), deprecated 매크로를 확인한다.
- `\begin{aligned}`, `\begin{array}`, `\underbrace`, `\overbrace`는 표준 매크로이므로 warning 대상이 아니다.
- `\begin{eqnarray}` 등 deprecated 매크로는 `report_only`로 기록한다.
```

## 완료 기준

- [ ] `validation_rules.json`에 `allowed_macros`가 추가되었다.
- [ ] `aligned`, `array`, `underbrace`, `overbrace`가 `discouraged_macros`에서 제거되었다.
- [ ] `discouraged_macros`에는 deprecated 또는 렌더링 문제 가능성이 높은 매크로만 남았다.
- [ ] `validation_guide.md`가 allowed/discouraged 구분을 반영한다.
- [ ] JSON 파싱 검증이 통과한다.

---

# Issue 5. PLAN 템플릿을 Mini/Full로 이원화

## 문제

현재 PLAN 파일은 모든 작업에 대해 과도하게 상세한 메타데이터 관리를 요구합니다. 매 차시마다 PLAN을 생성하고, 체크박스, 페이지 판독 기록, 입력 커버리지 검증, 실행 이슈, 잔여 이슈, Phase 1/2/3 등을 관리해야 합니다.

이는 긴 다중 입력 작업에는 유용하지만, 짧은 slide-only 작업이나 단순 수정 작업에서는 본문 작성보다 PLAN 관리에 더 많은 토큰을 쓰게 만들 수 있습니다.

## 수정 대상

- `agent_rules/memory_management_guideline.md`
- `agent_rules/note_creation_guide.md`
- `agent_rules/input_guide.md`
- `agent_rules/validation_guide.md`

## 요구 사항

### 1. PLAN 템플릿 종류 정의

`memory_management_guideline.md`에 다음 구조를 추가합니다.

```md
## PLAN 템플릿 종류

### Mini PLAN

적용 기준:
- 입력 자료가 `lecture_slide.pdf` 하나뿐이고 15페이지 이하인 경우
- STT/필기 없이 순수 슬라이드만으로 작성하는 경우
- 이미 작성된 노트의 소규모 갱신/수정인 경우
- 사용자가 명시적으로 “간단히”, “요약만”, “검토만”을 요청한 경우

필수 섹션:

```md
### 📋 목표: [과목명] [주차]-[차시]
- **타겟 출력 파일:** `output/과목명/과목명 주차-차시.md`
- **타겟 PLAN 파일:** `output/과목명/과목명_주차-차시_PLAN.md`

#### 진행 상태
- [x] 섹션 1: ...
- [ ] 섹션 2: ... (재개 지점)

#### ⚠️ 실행 이슈
- (발생 시 기록)

#### 잔여 이슈
- (발생 시 기록)
```

### Full PLAN

적용 기준:
- 손글씨 PDF + STT TXT + 슬라이드 PDF 등 다중 입력인 경우
- 20페이지 이상의 복잡한 필기 자료인 경우
- OCR 실패 가능성이 높은 스캔본인 경우
- 사용자가 장기 작업 재개 가능성 또는 상세 추적을 명시적으로 요구한 경우

필수 섹션:
- 기존 `memory_management_guideline.md`의 모든 주요 섹션을 유지한다.
- Phase 1/2/3, 페이지 판독 기록, 입력 커버리지 검증, 실행 이슈, 잔여 이슈를 포함한다.
```

주의: 위 Markdown 안의 중첩 코드블록은 실제 파일에 맞게 fence를 조정해 깨지지 않도록 작성합니다.

### 2. 체크박스 관리 단위 완화

`memory_management_guideline.md`의 UPDATE 원칙을 다음처럼 수정합니다.

```md
4. UPDATE (상태 업데이트 및 연속 진행)

- Mini PLAN: H2 섹션 완료 시에만 체크박스를 업데이트한다. H3 단위는 기록하지 않는다.
- Full PLAN: H3 단위까지 체크박스를 업데이트할 수 있으나, 연속 실행이 불가능해지는 경우에만 상세히 기록한다.
- 범위 대응이 불확실한 경우 사용자에게 즉시 질문하지 않는다. `[대응 불확실]` 등으로 기록하고 다음 섹션을 진행한다.
```

### 3. 페이지 판독 기록 선택화

`input_guide.md`의 페이지 판독 기록 규칙을 다음처럼 수정합니다.

```md
- Mini PLAN: 페이지 판독 기록은 선택 사항이다. 필요한 경우에만 `p.1~5: 핵심 개념 확인` 형태로 1줄 요약한다.
- Full PLAN: 페이지 단위 판독 기록을 권장하되, 연속 구간은 묶어 기록한다.
```

### 4. 검증 가이드에 PLAN 종류 반영

`validation_guide.md`의 입력 커버리지 검증에 다음을 반영합니다.

```md
- Mini PLAN에서는 입력 커버리지 검증을 구조 검증과 통합하여 수행한다.
- Full PLAN에서는 구조 검증과 입력 커버리지 검증을 분리하여 수행한다.
- slide-only 작업에서 Mini PLAN을 사용하는 경우 전체 슬라이드 검토 여부만 확인한다.
```

## 완료 기준

- [ ] `memory_management_guideline.md`에 Mini PLAN과 Full PLAN의 적용 기준이 정의되었다.
- [ ] Mini PLAN의 필수 섹션이 목표, 진행 상태, 실행 이슈, 잔여 이슈로 축약되었다.
- [ ] Full PLAN은 기존 상세 추적 구조를 유지한다.
- [ ] 체크박스 업데이트 단위가 Mini(H2) / Full(H3 가능)로 구분되었다.
- [ ] 페이지 판독 기록이 Mini(선택) / Full(권장)으로 구분되었다.
- [ ] `validation_guide.md`가 PLAN 종류별 검증 방식을 반영한다.

---

# Issue 6. manifest 상태 전이 트리거 명확화

## 문제

`input_acquisition_guide.md`는 manifest 상태 변경이 사용자가 명시적으로 문서 작성 작업을 요청한 경우에만 가능하다고 설명합니다. 하지만 자연어 요청에서 “작성”, “정리”, “요약”, “검토”의 경계가 모호할 수 있습니다.

예시:

- “열역학 3-1 정리해줘” → 파일 생성 작업일 가능성이 큼
- “이 내용 간단히 정리해줘” → 채팅 요약일 가능성이 큼
- “고체역학 이번 주 내용 보여줘” → 검토/조회일 가능성이 큼
- “이 PDF 읽고 요약해줘” → 문서 생성이 아니라 요약일 수 있음

## 수정 대상

- `agent_rules/input_acquisition_guide.md`
- `agent_rules/System Prompt Autonomous Markdown.md`

## 요구 사항

### 1. 사용자 동사별 작업 유형 매핑표 추가

`input_acquisition_guide.md`의 사용자 질문 정책 아래에 다음 표를 추가합니다.

```md
### 사용자 동사별 작업 유형 매핑

| 사용자 표현 | 작업 유형 | manifest 상태 변경 |
|---|---|---|
| 생성, 작성, 만들어, Markdown으로 만들어, 출력 파일로 만들어 | `new_note` | O (`pending` → `in_progress`) |
| 갱신, 업데이트, 수정, 고쳐, 보완 | `update_note` | O (`pending` → `in_progress`) |
| 강의노트로 정리, output에 정리, 파일로 정리 | `new_note` / `update_note` | O (`pending` → `in_progress`) |
| 검토, 분석, 점검, 확인해줘, 어떻게 생각해 | `review` | X (read-only) |
| 요약만, 간단히 설명, 대충 알려줘, 핵심만 알려줘 | `summary` | X (read-only) |
| dry-run, 테스트, 시뮬레이션 | `dry_run` | X (read-only) |
| PLAN만, 계획만 짜줘 | `planning_only` | X (본문 작성 없음; PLAN 생성/갱신은 허용) |

- 위 표에 없는 동사는 가장 유사한 유형으로 분류한다.
- 불확실하면 사용자에게 1회만 확인한다.
- `~해줘`로 끝나는 표현은 어미가 아니라 핵심 동사의 의미를 우선한다.
- `정리해줘`, `요약해줘`, `설명해줘`는 출력 파일 생성/수정 의도가 명시된 경우에만 `new_note` 또는 `update_note`로 분류한다.
- 출력 파일, 강의노트, Markdown, `output/` 경로가 언급되지 않으면 기본값은 `summary` 또는 `review`로 본다.
```

### 2. `task_type` 허용 값 명시

`input_acquisition_guide.md`에 다음 목록을 추가합니다.

```md
### task_type 허용 값

- `new_note`: 새 강의노트 생성
- `update_note`: 기존 강의노트 갱신/수정
- `review`: 검토 및 분석. manifest 상태 변경 없음
- `dry_run`: 실행 계획 및 검증. manifest 상태 변경 없음
- `planning_only`: PLAN 파일만 생성/갱신, 본문 작성 없음
- `summary`: 요약 및 설명. manifest 상태 변경 없음
```

### 3. System Prompt에 상태 변경 조건 명시

`System Prompt`의 최우선 실행 원칙 근처에 다음을 추가합니다.

```md
- 작업 유형은 `input_acquisition_guide.md`의 사용자 동사별 작업 유형 매핑표와 `task_type` 허용 값에 따라 결정한다.
- manifest 상태 변경은 `task_type`이 `new_note` 또는 `update_note`일 때만 허용한다.
- `review`, `dry_run`, `planning_only`, `summary`에서는 `inbox/`와 manifest를 read-only로 취급한다.
- 단, `planning_only`에서는 사용자가 명시적으로 요청한 경우 PLAN 생성/갱신만 허용한다.
```

## 완료 기준

- [ ] 사용자 동사별 작업 유형 매핑표가 추가되었다.
- [ ] `정리해줘`가 무조건 문서 작성으로 분류되지 않도록 조건이 명시되었다.
- [ ] `task_type` 허용 값 목록이 정의되었다.
- [ ] manifest 상태 변경은 `new_note` / `update_note`에서만 허용된다고 명시되었다.
- [ ] 기존 manifest 예시의 `task_type` 값이 허용 값 목록과 일치한다.

---

# Issue 7. 표준 워크플로우와 Tier 규칙 최종 정리

## 문제

`agent_rules`의 표준 워크플로우는 Guide E → F → A → B → C → D → G → A → E의 9단계로 되어 있어, 실제 문서 작성 작업에서 가이드 전환 비용이 큽니다. 또한 어떤 규칙이 필수(P0)이고 어떤 규칙이 권장(P1) 또는 참고(P2)인지 구분이 부족합니다.

단, 파일 전체를 P0/P1/P2로 분류하면 한 파일 안의 여러 성격의 규칙을 제대로 표현하기 어렵습니다. 따라서 파일 단위 Tier가 아니라 **섹션/규칙 단위 Tier**를 도입합니다.

## 수정 대상

- `agent_rules/System Prompt Autonomous Markdown.md`
- `agent_rules/memory_management_guideline.md`
- `agent_rules/codex_execution_guide.md`
- `agent_rules/note_creation_guide.md`
- `agent_rules/input_guide.md`
- `agent_rules/validation_guide.md`

## 요구 사항

### 1. 섹션/규칙 단위 Tier 도입

각 관련 가이드에서 핵심 섹션 제목 또는 규칙 앞에 `[P0]`, `[P1]`, `[P2]`를 붙입니다.

예시:

```md
### [P0] 위험 명령 제한

### [P0] 기존 사용자 변경 보호

### [P0] UTF-8 인코딩

### [P0] primary_scope_anchor 준수

### [P1] PLAN 파일 생성 및 갱신

### [P1] 페이지 판독 기록

### [P2] PLAN Phase 명칭

### [P2] 렌더링 캐시 보관 참고
```

파일 상단에는 필요한 경우 다음처럼 혼합 Tier임을 표시합니다.

```md
<!-- Guide Tier: Mixed -->
<!-- Contains: P0 safety rules, P1 workflow rules, P2 reference rules -->
```

파일 전체를 단일 P0로 태그하지 않습니다.

### 2. Tier 정의 중앙 정리

`System Prompt` 또는 `codex_execution_guide.md`에 다음 정의를 추가합니다.

```md
## Tier 정의

- P0 (필수): 위반 시 작업 실패 또는 안전 문제로 간주한다. 위험 명령 제한, 기존 변경 보호, UTF-8 인코딩, git 상태 확인, primary_scope_anchor 준수, hard rule 검증이 포함된다.
- P1 (권장): 가능한 한 준수하되, 위반 시 작업을 중단하지 않고 최종 리포트에 기록한다. PLAN 파일, 페이지 판독 기록, H2 구분선, 수식 블록 개행 등이 포함된다.
- P2 (참고): 작업 품질과 유지보수성을 높이는 참고 규칙이다. 상황에 따라 생략할 수 있으며, 위반만으로 이슈로 보지 않는다.
```

주의: H2 구분선이 Issue 2/3에서 hard rule로 유지되는 경우, 이 항목은 P0 또는 hard rule 목록과 일치하도록 조정합니다. 기존 규칙과 충돌하지 않게 최종 표현을 맞춥니다.

### 3. 표준 워크플로우 5단계 축약

`System Prompt`의 표준 작업 흐름을 다음처럼 축약합니다.

```md
## 표준 작업 흐름 — 문서화 작업에 한함

1. PREPARE (Guide E)
   - workspace/git 상태 확인
   - 기존 사용자 변경 보호
   - 위험 명령 점검

2. ACQUIRE (Guide F + A)
   - 작업 발견
   - manifest 확인
   - PLAN 읽기/생성
   - primary_scope_anchor 확인

3. EXECUTE (Guide B + C)
   - 입력 분석
   - 본문 작성 또는 갱신
   - 필요한 경우 PLAN 상태 업데이트

4. VALIDATE (Guide D)
   - JSON canonical source 기준 검증
   - hard rule / soft rule 분리
   - 입력 커버리지 검증

5. FINALIZE (Guide G + A + E)
   - 필요한 경우 웹 교차검증
   - PLAN/manifest 상태 반영
   - git diff 확인
   - 최종 보고
```

기존 9단계의 세부 내용은 각 단계 내부 작업으로 흡수합니다.

### 4. 가이드 재확인 기준 단순화

`codex_execution_guide.md`의 가이드 재확인 기준을 다음처럼 수정합니다.

```md
## 가이드 재확인 기준

가이드 재확인은 다음 3가지 경우에만 수행한다.

1. 작업 재개 또는 세션 복구 후
2. 검증 실패 원인 분석 시
3. 규칙 충돌이 의심될 때

그 외에는 같은 목적의 가이드를 반복 읽지 않는다.
```

## 완료 기준

- [ ] 파일 단위 단일 Tier가 아니라 섹션/규칙 단위 `[P0]`, `[P1]`, `[P2]`가 도입되었다.
- [ ] Tier 정의가 중앙 위치에 정리되었다.
- [ ] P0 규칙 목록에 위험 명령 제한, 기존 변경 보호, UTF-8 인코딩, git 상태 확인, primary_scope_anchor 준수가 포함된다.
- [ ] 표준 작업 흐름이 5단계로 축약되었다.
- [ ] 가이드 재확인 기준이 3가지 경우로 단순화되었다.
- [ ] 축약 과정에서 기존 안전 정책이 누락되지 않았다.

---

# Issue 8-A. 재요청 제한 및 task_priority fallback 완화

## 문제

재요청 제한과 작업 선택 fallback이 실제 사용성과 맞지 않는 부분이 있습니다.

- 판독 불가 시 재요청이 최대 2회로 고정되어 있어, 핵심 전제 내용에서도 추가 확인 여지가 없습니다.
- `task_priority`가 없을 때 “최신 수정 시간이 가장 오래된 작업부터 처리”하는 기준은 직관적이지 않습니다.

## 수정 대상

- `agent_rules/input_guide.md`
- `agent_rules/input_acquisition_guide.md`
- `agent_rules/memory_management_guideline.md`

## 요구 사항

### 1. 재요청 제한 완화

`input_guide.md`와 `memory_management_guideline.md`의 재요청 제한을 다음처럼 수정합니다.

```md
- 판독 불가 시 재요청은 기본 2회까지 허용한다.
- 단, 해당 내용이 이후 전개 전체의 핵심 전제라면 사용자에게 한 번 더 요청할 수 있다.
- 이 경우 PLAN에 `[핵심 전제: 추가 재요청 1회]`로 기록한다.
- 총 재요청 횟수는 3회를 초과하지 않는다.
```

### 2. `task_priority` fallback 변경

`input_acquisition_guide.md`의 작업 선택 우선순위 fallback을 다음처럼 수정합니다.

```md
- `task_priority`가 없고 `status: pending`인 작업이 여러 개면, manifest에 명시된 `subject`의 알파벳/가나다 순으로 처리한다.
- `subject`도 없으면 `input_root` 경로의 문자열 순서로 처리한다.
- 최신 수정 시간이 가장 오래된 작업을 자동으로 우선하지 않는다.
```

## 완료 기준

- [ ] 재요청이 기본 2회, 핵심 전제에 한해 최대 3회로 완화되었다.
- [ ] 추가 재요청 시 PLAN 기록 규칙이 추가되었다.
- [ ] `task_priority` fallback이 오래된 파일 기준에서 subject/path 문자열 순서로 변경되었다.

---

# Issue 8-B. Python 실행 환경 점검에서 `py` fallback 복원

## 문제

Windows 환경에서 `py` 런처는 일반적인 Python 실행 도구입니다. 현재 규칙이 `py`를 기본 검증에서 제외하거나 부정적으로 취급하면, 실제 사용 가능한 Python 환경을 놓칠 수 있습니다.

## 수정 대상

- `agent_rules/codex_execution_guide.md`

## 요구 사항

Python 실행 환경 점검 항목을 다음처럼 수정합니다.

```md
- 기본 검증 명령은 `conda run -n pdfkit310 python --version`을 우선 사용한다.
- `conda` 환경이 없거나 실패하면 `python --version`을 시도한다.
- `python`도 실패하면 `py --version`을 시도한다.
- `py --version` 실패는 Python 런처 환경 이슈로 기록하되, 이를 이유로 PDF/OCR 작업을 즉시 중단하지 않는다.
- 실제 작업에 사용할 Python 실행 명령은 성공한 검증 결과를 기준으로 선택한다.
```

## 완료 기준

- [ ] `py --version`이 Python 검증 fallback 순서에 포함되었다.
- [ ] `py` 실패만으로 PDF/OCR 작업을 중단하지 않는다고 명시되었다.
- [ ] 실제 작업 명령은 성공한 검증 결과를 기준으로 선택한다고 명시되었다.

---

# Issue 8-C. PDF 렌더링 캐시 보관 정책 수정

## 문제

기본 모드에서 PDF 판독용 렌더링 이미지를 즉시 삭제하면, 판독 오류 재검토나 디버깅이 어려워집니다. 반대로 무기한 보관하면 저장 공간이 불필요하게 늘어날 수 있습니다.

## 수정 대상

- `agent_rules/codex_execution_guide.md`

## 요구 사항

렌더링 이미지 캐시 보관 정책을 다음처럼 수정합니다.

```md
- 기본 모드: PDF 판독용 렌더링 이미지를 작업 완료 후 즉시 삭제하지 않고 24시간 보관 대상으로 둔다.
- 디버그 모드: 렌더링 이미지를 `output/_render_cache/{subject}/{lecture_or_unit}/` 아래에 보관한다.
- 사용자가 “캐시를 지워줘”라고 요청하면 기본 모드와 디버그 모드의 관련 캐시를 삭제할 수 있다.
- 캐시 보관은 원본 PDF나 최종 Markdown 산출물의 보관 정책을 변경하지 않는다.
```

## 완료 기준

- [ ] 기본 모드에서 렌더링 이미지를 즉시 삭제하지 않고 24시간 보관 대상으로 둔다.
- [ ] 디버그 모드 캐시 경로가 명시되어 있다.
- [ ] 사용자 요청 시 캐시 삭제 가능하다고 명시되어 있다.
- [ ] 원본 PDF/최종 산출물 보관 정책과 캐시 정책이 구분되어 있다.

---

# Issue 8-D. PLAN 언어 기본값 정리

## 문제

PLAN 파일에 영어와 한국어가 섞일 수 있지만, 기본 원칙이 없으면 한 섹션 안에서도 언어가 불규칙하게 바뀌어 가독성이 떨어질 수 있습니다.

## 수정 대상

- `agent_rules/memory_management_guideline.md`

## 요구 사항

PLAN Language Policy에 다음 기본값을 추가합니다.

```md
## PLAN Language Policy

- PLAN files may mix English and Korean.
- 기본값: 기술적 워크플로우(체크박스, Phase 이름, 상태명)는 영어로 작성한다.
- 기본값: 과목 주제, 개념 해석, 판독 메모는 한국어로 작성한다.
- 특별한 이유 없이 한 섹션 안에서 언어를 반복적으로 번갈아 쓰지 않는다.
- 사용자가 특정 언어 스타일을 요청하면 그 요청을 우선한다.
```

## 완료 기준

- [ ] PLAN 언어 정책에 기본값이 추가되었다.
- [ ] 기술적 워크플로우는 영어, 과목 주제/해석 노트는 한국어로 작성한다는 기준이 명시되었다.
- [ ] 한 섹션 안에서 불필요한 언어 혼용을 피한다는 원칙이 추가되었다.
- [ ] 사용자 요청이 있을 경우 해당 요청을 우선한다고 명시되었다.
