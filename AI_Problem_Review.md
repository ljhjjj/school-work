# Issue 1. 규칙 복잡도 폭발 (Cognitive Overload)

## 문제

`agent_rules`에 총 11개 파일(8개 Markdown + 3개 JSON), 1,300줄 이상의 규칙이 존재합니다. 표준 워크플로우는 Guide E → F → A → B → C → D → G → A → E의 9단계로, 한 번의 강의노트 생성마다 여러 가이드를 읽고 PLAN까지 갱신해야 합니다.

System Prompt에 "Trigger Narrowing"과 단발성 요청 예외 처리가 있지만, **실제 문서 작성 작업이 시작되면** AI는 여전히 1,300줄이 넘는 규칙 중 어떤 것이 필수이고 어떤 것이 부수적인지 구분하기 어렵습니다. 이로 인해 핵심 안전 정책(위험 명령 제한, 기존 변경 보호)과 부수적 정책(PLAN 섹션 구조, 페이지 판독 기록 형식)이 동일한 우선순위로 취급되어 실행 품질이 저하될 수 있습니다.

## 수정 대상

- `agent_rules/System Prompt Autonomous Markdown.md`
- `agent_rules/memory_management_guideline.md`
- `agent_rules/codex_execution_guide.md`
- `agent_rules/note_creation_guide.md`
- `agent_rules/input_guide.md`
- `agent_rules/validation_guide.md`

## 요구 사항

### 1. Tier 기반 규칙 우선순위 도입

각 가이드 파일 상단에 반드시 아래 형식의 Tier 태그를 추가합니다.

```md
<!-- Tier: P0 -->
<!-- 이 가이드의 규칙은 작업 안전성과 산출물 품질에 필수적입니다. 생략할 수 없습니다. -->
```

Tier 정의는 다음과 같습니다.

- **P0 (필수)**: 위험 명령 제한, 기존 변경 보호, UTF-8 인코딩, git 상태 확인, `primary_scope_anchor` 준수 등. 이를 위반하면 작업 실패로 간주합니다.
- **P1 (권장)**: PLAN 파일 생성, 페이지 판독 기록, H2 구분선, 수식 블록 개행 등. 위반 시 작업은 계속하되 최종 리포트에 기록합니다.
- **P2 (참고)**: 페이지 판독 기록의 상세 형식, PLAN의 Phase 구분 명칭, 렌더링 캐시 보관 여부 등. 위반 시 무시하고 진행합니다.

### 2. 표준 워크플로우 단순화

`System Prompt`의 "표준 작업 흐름"을 9단계에서 5단계로 축약합니다.

```md
## ⚙️ 표준 작업 흐름 (Standard Workflow) — 문서화 작업에 한함

1. **PREPARE** (Guide E) — workspace/git 상태 확인, 위험 명령 점검
2. **ACQUIRE** (Guide F + A) — 작업 발견, manifest 확인, PLAN 읽기/생성
3. **EXECUTE** (Guide B + C) — 입력 분석 및 본문 작성
4. **VALIDATE** (Guide D) — 구조 검증 및 입력 커버리지 검증
5. **FINALIZE** (Guide G + A + E) — 웹 교차검증, PLAN 상태 반영, git diff 및 최종 보고
```

기존 9단계의 세부 내용은 각 단계 내부에서 처리하며, 별도의 외부 가이드 전환 없이 진행합니다.

### 3. 가이드 재확인 기준 명확화

`codex_execution_guide.md`의 "가이드 재확인 기준"을 다음과 같이 단순화합니다.

```md
- 가이드 재확인은 3가지 경우에만 수행합니다.
  1. 작업 재개 또는 세션 복구 후
  2. 검증 실패 원인 분석 시
  3. 규칙 충돌이 의심될 때
- 그 외에는 같은 목적의 가이드를 반복 읽지 않습니다.
```

## 완료 기준

- [ ] 모든 가이드 파일 상단에 `<!-- Tier: P0/P1/P2 -->` 태그가 추가되어 있다.
- [ ] `System Prompt`의 표준 작업 흐름이 5단계로 축약되어 있다.
- [ ] P0 규칙 목록이 `System Prompt` 또는 `codex_execution_guide.md`에 중앙 집중식으로 정리되어 있다.
- [ ] 가이드 재확인 기준이 3가지 경우로 단순화되어 있다.
- [ ] 축약 과정에서 기존 안전 정책(위험 명령, git 체크, 인코딩)이 누락되지 않았다.

---

# Issue 2. JSON과 Markdown의 이중성 및 중복

## 문제

동일한 내용이 JSON 스키마와 Markdown 서술에 중복/분산되어 있습니다.

- `dangerous_commands`: `codex_execution_guide.md` 5.2절에 서술되어 있으면서도 `schemas/codex_policy.json`에 동일하게 구조화되어 있습니다.
- `report_templates`: `codex_execution_guide.md` 8.2절에 언급되면서도 `schemas/report_templates.json`에 존재합니다.
- `validation` 체크항목: `validation_guide.md`에 서술되어 있으면서도 `schemas/validation_rules.json`에 존재합니다.

이로 인해 규칙 변경 시 2개 이상의 파일을 동시에 수정해야 하며, 누락 시 AI가 혼란을 겪습니다. 또한 JSON 파일 누락 시 "기본 지식으로 대체 실행"하는 fallback 정책은 일관성 없는 동작을 유발합니다.

## 수정 대상

- `agent_rules/codex_execution_guide.md`
- `agent_rules/validation_guide.md`
- `agent_rules/input_acquisition_guide.md`
- `agent_rules/schemas/codex_policy.json`
- `agent_rules/schemas/validation_rules.json`
- `agent_rules/schemas/report_templates.json`

## 요구 사항

### 1. JSON을 canonical source로 선언

`System Prompt` 또는 `codex_execution_guide.md`에 다음 원칙을 명시합니다.

```md
- `schemas/` 폴더의 JSON 파일은 해당 도메인의 **유일한 기계 해석 가능한 규칙(canonical source)**입니다.
- Markdown 가이드는 "왜 그런지", "언제 적용하는지"에 대한 설명과 맥락만 제공합니다.
- JSON과 Markdown이 충돌할 경우 **JSON이 우선**합니다.
```

### 2. Markdown 중복 서술 제거

`codex_execution_guide.md`의 위험 명령 제한(5.2절)을 다음과 같이 변경합니다.

```md
### 5.2 위험 명령 제한
- 위험 명령 분류 및 정책은 `schemas/codex_policy.json`의 `dangerous_commands`를 참조한다.
- 조회 전용 명령(`git status`, `git diff` 등)과 변경 명령의 구분 기준은 해당 JSON의 `read_only_commands`와 `requires_explicit_user_approval`을 따른다.
```

기존의 8가지 카테고리별 상세 서술은 제거하고 JSON 참조로 대체합니다.

`validation_guide.md`의 Required Checks도 마찬가지입니다.

```md
## 3. Required Checks
- 구조 검증 항목은 `schemas/validation_rules.json`의 `markdown_structure`, `latex`, `input_coverage`를 따른다.
- 파일 경로 및 인코딩 규칙은 해당 JSON의 `paths`, `encoding`을 따른다.
- 태그 분류 기준은 `normal_status_tags`와 `residual_issue_tags`를 따른다.
```

### 3. report_templates 참조 단순화

`codex_execution_guide.md` 8.2절을 다음과 같이 변경합니다.

```md
### 8.2 작업 요약 및 최종 보고 형식
- 최종 리포트는 `schemas/report_templates.json`의 템플릿을 선택한다.
- 템플릿 선택 기준은 해당 JSON의 `template_selection`을 따른다.
- 템플릿이 누락된 경우에는 작업 유형에 맞는 일반 리포트 양식으로 간결하게 자체 출력한다.
```

기존의 `lecture_note_execution_summary`, `rule_review_summary` 등 개별 템플릿 서술은 제거합니다.

### 4. JSON fallback 정책 보완

`codex_execution_guide.md` 3.4절을 다음과 같이 보완합니다.

```md
- 파일 누락이 확인되면, 에이전트의 자체적인 기본 지식을 바탕으로 작업을 강행한다.
- 단, 기본 지식과 기존 JSON 규칙이 충돌할 가능성이 있으므로, 최대한 JSON의 구조(예: `dangerous_commands`의 카테고리 이름, `validation_rules`의 검증 항목 이름)를 기억하여 일관성 있게 적용한다.
- 작업 완료 후 `[⚠️ 실행 이슈]`에 JSON 누락 사실과 함께 **"어떤 JSON 규칙을 기본 지식으로 대체했는지"**를 구체적으로 명시한다.
```

## 완료 기준

- [ ] `System Prompt` 또는 `codex_execution_guide.md`에 "JSON이 canonical source이며 충돌 시 우선"이라는 원칙이 명시되어 있다.
- [ ] `codex_execution_guide.md`의 위험 명령 제한(5.2절)이 JSON 참조로 대체되어 중복 서술이 제거되었다.
- [ ] `validation_guide.md`의 Required Checks가 JSON 참조로 대체되어 중복 서술이 제거되었다.
- [ ] `codex_execution_guide.md`의 보고 형식(8.2절)이 JSON 참조로 대체되어 중복 서술이 제거되었다.
- [ ] JSON 누락 fallback 정책이 구체화되어 있다.
- [ ] 수정 후 관련 JSON 파일의 파싱이 정상적으로 통과한다.

---

# Issue 3. 정규식 검증 규칙의 경직성

## 문제

`schemas/validation_rules.json`의 `markdown_structure`가 너무 엄격하여 다양한 문서 형식에 대응하지 못합니다.

- H1: `^# .+ \d+-\d+: .+$` → 반드시 "과목명 주차-차시: 주제" 형식만 허용
- H2: `^## \d+\. .+` → 반드시 숫자로 시작해야 함
- H3: `^### \d+\) .+` → 반드시 숫자+`)`로 시작해야 함

이는 특강, 세미나, 실습, 요약형 문서 등에서 "숫자-차시" 개념이 없거나 섹션 제목에 자유로운 명칭을 사용할 때 검증 실패를 유발합니다. (단, `\d+`는 두 자리 주차도 커버하고 `.+`는 특수문자도 커버하므로 자릿수 문제는 아닙니다.)

## 수정 대상

- `agent_rules/schemas/validation_rules.json`
- `agent_rules/validation_guide.md`

## 요구 사항

### 1. H1 패턴 완화

`validation_rules.json`의 `h1.pattern`을 다음과 같이 변경합니다.

```json
"h1": {
  "required_count": 1,
  "pattern": "^# .+$",
  "recommended_pattern": "^# .+ \\d+-\\d+: .+$"
}
```

- `pattern`은 `^# .+$`로 완화하여 **임의의 H1 제목**을 허용합니다.
- `recommended_pattern`은 기존의 "과목명 주차-차시: 주제" 형식을 **권장 규칙**으로 유지합니다.
- `validation_guide.md`에서 H1 검증 실패는 hard failure가 아닌 soft warning으로 처리합니다.

### 2. H2/H3 숫자 제목 완화

`validation_rules.json`의 `h2`와 `h3`를 다음과 같이 변경합니다.

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

- `pattern`은 `^## .+`와 `^### .+`로 완화하여 임의의 제목을 허용합니다.
- `recommended_pattern`은 기존 숫자 제목 형식을 권장 규칙으로 유지합니다.
- H2의 `require_horizontal_rule_at_section_end`는 hard rule로 유지합니다(문서 구조 일관성을 위해).

### 3. hard rule / soft rule 분리

`validation_rules.json`에 최상위 레벨로 `severity` 필드를 추가합니다.

```json
{
  "severity": {
    "hard_rules": ["paths", "encoding", "h1_required_count", "h2_horizontal_rule"],
    "soft_rules": ["h1_pattern", "h2_pattern", "h3_pattern", "latex_discouraged_macros"]
  }
}
```

- **hard rule 위반**: 작업 완료로 인정하지 않고 자동 수정을 시도하거나 사용자에게 알립니다.
- **soft rule 위반**: 최종 리포트에 기록하되 작업을 계속 진행합니다.

## 완료 기준

- [ ] `validation_rules.json`의 H1 패턴이 `^# .+$`로 완화되고, 기존 패턴은 `recommended_pattern`으로 이동되었다.
- [ ] H2/H3 패턴이 `^## .+` / `^### .+`로 완화되고, 기존 숫자 패턴은 `recommended_pattern`으로 이동되었다.
- [ ] `severity` 필드가 추가되어 hard rule과 soft rule이 구분되었다.
- [ ] `validation_guide.md`가 hard/soft 구분을 반영하여 업데이트되었다.
- [ ] 수정된 JSON이 파싱 검증을 통과한다.

---

# Issue 4. LaTeX 매크로 제한의 과도함

## 문제

`schemas/validation_rules.json`의 `latex.discouraged_macros`에 다음이 포함되어 있습니다.

- `\underbrace`
- `\overbrace`
- `\begin{array}`
- `\begin{aligned}`

이들은 공학/수학 강의노트에서 **매우 일반적으로 사용**되는 표준 매크로입니다. 특히 `aligned` 환경은 다중 행 수식의 기본이며, `array`는 행렬이나 정렬된 수식 체계에 필수적입니다. 현재 `severity: warning`, `action: report_only`로 설정되어 있어 본문 생성을 차단하지는 않지만, 검증 리포트에 불필요한 노이즈를 발생시켜 다른 진짜 이슈를 묻어버릴 수 있습니다.

## 수정 대상

- `agent_rules/schemas/validation_rules.json`

## 요구 사항

### 1. 표준 매크로 허용 목록 추가

`validation_rules.json`의 `latex` 섹션에 `allowed_macros`를 추가하고, 기존 `discouraged_macros`를 재검토합니다.

```json
"latex": {
  "inline_pattern": "\\$[^$\\n]+\\$",
  "block_delimiter": "$$",
  "block_must_be_standalone": true,
  "forbidden_patterns": [
    ".*\\$\\$[^\\n]+\\$\\$.*",
    "<math[\\s>]",
    "</math>"
  ],
  "allowed_macros": [
    { "pattern": "\\\\begin\\{aligned\\}", "note": "standard multi-line equation environment" },
    { "pattern": "\\\\begin\\{array\\}", "note": "standard tabular math environment" },
    { "pattern": "\\\\underbrace", "note": "standard annotation macro" },
    { "pattern": "\\\\overbrace", "note": "standard annotation macro" }
  ],
  "discouraged_macros": [
    {
      "pattern": "\\\\begin\\{eqnarray\\}",
      "severity": "warning",
      "action": "report_only",
      "reason": "deprecated, use aligned instead"
    }
  ]
}
```

- `aligned`, `array`, `underbrace`, `overbrace`는 `allowed_macros`로 이동하여 검증 warning 대상에서 제외합니다.
- `eqnarray`처럼 실제로 deprecated된 매크로만 `discouraged_macros`에 남깁니다.

### 2. `validation_guide.md` 반영

`validation_guide.md`의 LaTeX 검증 항목을 다음과 같이 수정합니다.

```md
- `latex` 규칙에 따라 인라인/블록 수식, 금지 패턴(`<math>` 태그 등), 그리고 deprecated 매크로를 확인한다.
- `\begin{aligned}`, `\begin{array}`, `\underbrace`, `\overbrace`는 표준 매크로이므로 warning 대상이 아니다.
- `\begin{eqnarray}` 등 deprecated 매크로가 발견되면 `report_only`로 기록한다.
```

## 완료 기준

- [ ] `validation_rules.json`에 `allowed_macros` 항목이 추가되었다.
- [ ] `aligned`, `array`, `underbrace`, `overbrace`가 `discouraged_macros`에서 제거되었다.
- [ ] `discouraged_macros`에는 실제로 deprecated된 매크로만 남아 있다.
- [ ] `validation_guide.md`가 허용/비허용 매크로 구분을 반영하여 업데이트되었다.
- [ ] 수정된 JSON이 파싱 검증을 통과한다.

---

# Issue 5. manifest 상태 전이 트리거의 모호성

## 문제

`input_acquisition_guide.md`는 manifest 상태 변경이 "사용자가 **명시적으로** 문서 작성 작업을 요청한 경우"에만 가능하다고 명시하고 있습니다. 하지만 AI가 사용자의 자연어 요청이 "명시적 요청"인지 "암묵적 요청"인지를 판단하는 것은 여전히 주관적입니다.

예시:
- "열역학 3-1 정리해줘" → 명시적? 암묵적?
- "고체역학 이번 주 내용 보여줘" → 검토인가 작성인가?
- "이 PDF 읽고 요약해줘" → 요약은 작성인가 검토인가?

이로 인해 의도치 않게 `pending` → `in_progress`로 전환되거나, 실제 작성 요청임에도 상태가 변경되지 않을 수 있습니다.

## 수정 대상

- `agent_rules/input_acquisition_guide.md`
- `agent_rules/System Prompt Autonomous Markdown.md`

## 요구 사항

### 1. 사용자 동사-작업 유형 매핑표 추가

`input_acquisition_guide.md`의 "사용자 질문 정책" 아래에 다음 표를 추가합니다.

```md
### 사용자 동사별 작업 유형 매핑

| 사용자 표현 | 작업 유형 | manifest 상태 변경 |
|-----------|----------|------------------|
| 생성, 작성, 정리, 변환, 만들어, 변환해 | `new_note` / `update_note` | O (pending → in_progress) |
| 갱신, 업데이트, 수정, 고쳐 | `update_note` | O (pending → in_progress) |
| 검토, 분석, 점검, 확인해줘, 어떻게 생각해 | `review` | X (read-only) |
| 요약만, 간단히 설명, 대충 알려줘 | `summary` | X (read-only) |
| dry-run, 테스트, 시뮬레이션 | `dry_run` | X (read-only) |
| PLAN만, 계획만 짜줘 | `planning_only` | X (read-only, PLAN 생성은 허용) |

- 위 표에 없는 동사가 나오면 **가장 유사한 유형**으로 분류하되, 불확실하면 사용자에게 1회만 확인한다.
- "~해줘"로 끝나는 표현은 동사의 의미를 우선으로 본다.
```

### 2. `task_type` 필드 확장

`input_acquisition_guide.md`의 manifest 예시들에 `task_type`이 이미 존재하지만, 허용 값 목록을 명시적으로 정의합니다.

```md
### task_type 허용 값

- `new_note`: 새 강의노트 생성
- `update_note`: 기존 강의노트 갱신/수정
- `review`: 검토 및 분석 (manifest 상태 변경 없음)
- `dry_run`: 실행 계획 및 검증 (manifest 상태 변경 없음)
- `planning_only`: PLAN 파일만 생성/갱신, 본문 작성 없음 (manifest 상태 변경 없음)
- `summary`: 요약 및 설명 (manifest 상태 변경 없음)
```

### 3. System Prompt에 트리거 명시

`System Prompt`의 "0. 최우선 실행 원칙" 아래에 다음을 추가합니다.

```md
- **작업 유형 판단 기준:** 사용자의 동사를 `input_acquisition_guide.md`의 "사용자 동사별 작업 유형 매핑"표와 대조하여 작업 유형을 결정한다.
- **manifest 상태 변경 허용:** `task_type`이 `new_note` 또는 `update_note`일 때만 manifest 상태를 변경할 수 있다.
- **그 외 유형:** `review`, `dry_run`, `planning_only`, `summary` 등은 `inbox/`와 manifest를 read-only로 취급하며, 필요한 변경은 제안만 보고한다.
```

## 완료 기준

- [ ] `input_acquisition_guide.md`에 사용자 동사-작업 유형 매핑표가 추가되었다.
- [ ] `task_type` 허용 값 목록이 명시적으로 정의되었다.
- [ ] `System Prompt`에 작업 유형 판단 기준과 manifest 상태 변경 허용 조건이 추가되었다.
- [ ] 기존 manifest 예시의 `task_type` 값이 허용 값 목록과 일치한다.

---

# Issue 6. PLAN 파일 관리의 현실적 부담

## 문제

매 차시마다 `output/과목명/과목명_주차-차시_PLAN.md`를 생성하고, 체크박스(`[x]`/`[ ]`)를 수동 업데이트하며, `페이지 판독 기록`, `입력 커버리지 검증`, `⚠️ 실행 이슈`, `잔여 이슈`, `Phase 1/2/3` 등의 섹션을 모두 관리해야 합니다.

이는 AI의 자율성을 표방하면서도 실제로는 엄청난 메타데이터 관리를 요구하는 모순입니다. 본문 작성보다 PLAN 파일 업데이트에 더 많은 토큰을 소모할 수 있으며, 작업 재개 시 PLAN의 체크박스 상태와 실제 파일 내용이 불일치할 위험이 큽니다.

## 수정 대상

- `agent_rules/memory_management_guideline.md`
- `agent_rules/note_creation_guide.md`
- `agent_rules/validation_guide.md`

## 요구 사항

### 1. PLAN 템플릿 이원화 (mini / full)

`memory_management_guideline.md`에 PLAN의 두 가지 템플릿을 정의합니다.

```md
## PLAN 템플릿 종류

### Mini PLAN (간단 작업용)

적용 기준:
- 입력 자료가 `lecture_slide.pdf` 하나뿐이고 15페이지 이하
- STT/필기 없이 순수 슬라이드만으로 작성
- 이미 작성된 노트의 소규모 갱신/수정

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

### Full PLAN (복잡 작업용)

적용 기준:
- 손글씨 PDF + STT TXT + 슬라이드 PDF 등 다중 입력
- 20페이지 이상의 복잡한 필기 자료
- OCR 실패 가능성이 높은 스캔본

필수 섹션:
- 기존 `memory_management_guideline.md`의 모든 섹션(Phase 1/2/3, 페이지 판독 기록, 입력 커버리지 검증 등)을 그대로 유지
```

### 2. 체크박스 관리 완화

`memory_management_guideline.md`의 "UPDATE" 원칙을 수정합니다.

```md
4. **UPDATE (상태 업데이트 및 연속 진행):**
   - **Mini PLAN:** H2 섹션 완료 시에만 체크박스를 업데이트한다. H3 단위는 기록하지 않는다.
   - **Full PLAN:** H3 단위까지 체크박스를 업데이트할 수 있으나, 연속 실행이 불가능해지는 경우에만 상세히 기록한다.
   - 범위 대응이 불확실한 경우 사용자에게 즉시 질문하지 않는다. `[대응 불확실]` 등으로 기록하고 다음 섹션을 진행한다.
```

### 3. 페이지 판독 기록 선택화

`input_guide.md`의 "페이지 판독 기록" 규칙을 수정합니다.

```md
- **Mini PLAN:** 페이지 판독 기록은 선택 사항이다. 필요한 경우에만 `p.1~5: 핵심 개념 확인` 형태로 1줄 요약한다.
- **Full PLAN:** 페이지 단위 판독 기록을 권장하되, 연속 구간은 묶어 기록한다.
```

### 4. 검증 가이드에서 PLAN 상세도 구분

`validation_guide.md`의 입력 커버리지 검증을 수정합니다.

```md
- Mini PLAN에서는 입력 커버리지 검증을 `구조 검증`과 통합하여 수행한다.
- Full PLAN에서는 `구조 검증`과 `입력 커버리지 검증`을 분리하여 수행한다.
- slide-only 작업(Mini PLAN 해당)에서는 전체 슬라이드 검토 여부만 확인한다.
```

## 완료 기준

- [ ] `memory_management_guideline.md`에 Mini PLAN과 Full PLAN의 적용 기준과 필수 섹션이 정의되었다.
- [ ] Mini PLAN의 필수 섹션이 4개(목표, 진행상태, 실행이슈, 잔여이슈)로 축약되었다.
- [ ] 체크박스 업데이트 단위가 Mini(H2) / Full(H3)으로 구분되었다.
- [ ] 페이지 판독 기록이 Mini(선택) / Full(권장)으로 구분되었다.
- [ ] `validation_guide.md`가 PLAN 종류별 검증 방식을 반영하여 업데이트되었다.

---

# Issue 7. 기타 미세 문제

## 문제

다음과 같은 소규모 규칙들이 실제 사용성을 저해하거나 불필요한 제약을 가하고 있습니다.

| 문제 | 위치 | 현재 상태 | 영향 |
|------|------|----------|------|
| 재요청 2회 제한 | `input_guide.md`, `memory_management_guideline.md` | 판독 불가 시 최대 2회까지만 재요청 허용 | 중요한 핵심 전제 내용의 경우 사용자가 더 많은 재시도를 원할 수 있음 |
| `task_priority` 없을 시 fallback | `input_acquisition_guide.md` | 최신 수정 시간이 가장 오래된 작업부터 처리 | 직관적이지 않으며, 오래된 파일이 먼저 처리되어야 할 이유가 없음 |
| `py` 런처 제한 | `codex_execution_guide.md` | Windows `py` 런처를 기본 검증에서 제외 | Python 표준 도구인 `py`를 사용할 수 없음 |
| 렌더링 캐시 기본 삭제 | `codex_execution_guide.md` | 기본 모드에서 PDF 렌더링 이미지 작업 종료 후 삭제 | 판독 오류 재검토 시 원본 이미지가 없어 디버깅 어려움 |
| PLAN 언어 혼용 | `memory_management_guideline.md` | PLAN 파일에 영어/한국어 혼용 허용 | 일관된 언어 스타일 유지 어려움 |

## 수정 대상

- `agent_rules/input_guide.md`
- `agent_rules/input_acquisition_guide.md`
- `agent_rules/memory_management_guideline.md`
- `agent_rules/codex_execution_guide.md`

## 요구 사항

### 1. 재요청 횟수 완화

`input_guide.md`와 `memory_management_guideline.md`의 재요청 제한을 다음과 같이 수정합니다.

```md
- **[재요청 제한 및 강제 진행]:** 판독 불가 시 재요청은 **기본 2회**까지 허용된다.
- 단, 해당 내용이 **이후 전개 전체의 핵심 전제**라면 사용자에게 한 번 더 요청할 수 있으며, 이 경우 PLAN에 `[핵심 전제: 추가 재요청 1회]`로 기록한다.
- 총 재요청 횟수는 **3회를 초과하지 않는다**.
```

### 2. `task_priority` 없을 시 fallback 변경

`input_acquisition_guide.md`의 "작업 선택 우선순위" 4번 항목을 수정합니다.

```md
4. `task_priority`가 없고 `status: pending`인 작업이 여러 개면, manifest에 명시된 `subject`의 알파벳/가나다 순으로 처리한다. `subject`도 없으면 `input_root` 경로의 문자열 순서로 처리한다.
```

기존의 "최신 수정 시간이 가장 오래된 작업" 기준은 제거합니다.

### 3. `py` 런처 기본 검증 복원

`codex_execution_guide.md`의 Python 실행 환경 점검 항목을 수정합니다.

```md
- 기본 검증 명령은 `conda run -n pdfkit310 python --version`을 우선 사용한다.
- `conda` 환경이 없는 경우 `python --version` 또는 `py --version`을 순차적으로 시도한다.
- `py --version` 실패는 Python 런처 환경 이슈로 기록하되, 이를 이유로 PDF/OCR 작업을 중단하지는 않는다.
```

### 4. 렌더링 캐시 기본 보관 기간 설정

`codex_execution_guide.md`의 렌더링 이미지 캐시 보관 모드를 수정합니다.

```md
- **기본 모드:** PDF 판독용 렌더링 이미지를 **24시간** 보관한 후 삭제한다.
- **디버그 모드:** 렌더링 이미지를 `output/_render_cache/{subject}/{lecture_or_unit}/` 아래에 보관한다.
- 사용자가 "캐시를 지워줘"라고 요청할 때까지 기본 모드에서도 즉시 삭제하지 않는다.
```

### 5. PLAN 언어 기본값 설정

`memory_management_guideline.md`의 PLAN Language Policy에 기본값을 추가합니다.

```md
- PLAN files may mix English and Korean.
- **기본값:** 기술적 워크플로우(체크박스, Phase 이름, 상태)는 영어로, 과목 주제와 해석 노트는 한국어로 작성한다.
- 특별한 이유 없이 한 섹션 내에서 언어를 번갈아 쓰지 않는다.
```

## 완료 기준

- [ ] 재요청 횟수가 핵심 전제에 따라 1회 추가 가능하도록 완화되었다.
- [ ] `task_priority` 없을 시 fallback이 "오래된 파일"에서 "알파벳/가나다 순"으로 변경되었다.
- [ ] `py` 런처가 기본 검증의 fallback 순서에 포함되었다.
- [ ] 렌더링 캐시 기본 모드가 24시간 보관으로 변경되었다.
- [ ] PLAN 언어에 기본값(기술=영어, 주제=한국어)이 추가되었다.

---

# Codex에 같이 붙일 공통 지시

```text
수정 전 관련 파일을 먼저 읽고 현재 섹션 구조를 확인할 것.
기존 규칙의 표현 방식과 문체를 최대한 유지할 것.
한 Issue만 처리하고, 다른 Issue의 내용은 불필요하게 섞지 말 것.
JSON 파일을 수정한 경우 PowerShell ConvertFrom-Json 또는 동등한 JSON 파서로 검증할 것.
작업 후 git status --short와 변경 파일 요약을 보고할 것.
수정 사항이 기존 안전 정책(위험 명령 제한, 기존 변경 보호, UTF-8 인코딩)을 약화시키지 않도록 확인할 것.
```
