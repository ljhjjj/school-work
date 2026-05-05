# agent_rules 개선 Issue 요청서

> 목적: `inbox/열역학/8-2` 작업 중 확인된 실행/범위/검증 문제를 `agent_rules/`에 반영하여 후속 강의노트 작업의 판단 안정성을 높인다.
> 범위: **루트 `AGENTS.md` 수정은 제외한다.**
> 권장 사용 방식: **한 번에 하나의 Issue만** 붙여서 처리한다.
> 원칙: P0 안전 정책(기존 변경 보호, 위험 명령 제한, UTF-8 인코딩, primary_scope_anchor 준수)을 약화하지 않는다.
> 권장 처리 순서: Issue 1 → 2 → 3 → 4 → 5 → 6

---

# 공통 지시

```text
수정 전 관련 파일을 먼저 읽고 현재 섹션 구조를 확인할 것.
루트 AGENTS.md는 수정하지 말 것.
기존 규칙의 표현 방식과 문체를 최대한 유지할 것.
한 Issue만 처리하고, 다른 Issue의 내용은 불필요하게 섞지 말 것.
Issue 요구사항과 충돌하지 않는 기존 P0 안전 정책은 절대 삭제하거나 약화하지 말 것.
새 파일을 만들거나 기존 Markdown/JSON 파일을 수정할 경우 UTF-8로 저장할 것.
JSON 파일을 수정한 경우 PowerShell ConvertFrom-Json 또는 동등한 JSON 파서로 검증할 것.
작업 후 git status --short와 변경 파일 요약을 보고할 것.
수정 사항이 기존 안전 정책(위험 명령 제한, 기존 변경 보호, UTF-8 인코딩, primary_scope_anchor 준수)을 약화시키지 않도록 확인할 것.

토큰 절약 및 파일 접근 제한:
- 수정 대상 파일과 직접 참조해야 하는 schema/index 파일만 읽을 것.
- 같은 파일을 전체 ReadFile로 2회 이상 읽지 말 것.
- 재확인이 필요하면 전체 ReadFile 대신 검색 또는 해당 섹션만 확인할 것.
- 다른 Issue의 요구사항은 분석하지 말고 수정하지 말 것.
```

---

# Issue 1. `STT TXT + 강의자료 PDF` scope anchor 규칙 추가

## 배경

`inbox/열역학/8-2` 작업 입력은 다음 조합이었다.

- `lecture_stt_20260503_153054.txt`
- `lecture_slide(1).pdf`
- `lecture_slide(2).pdf`

현재 `agent_rules/input/scope_anchor_rules.md`에는 다음 조합은 명시되어 있다.

- 손글씨 PDF + 음성 TXT + 강의자료 PDF
- 필기 PDF + 강의자료 PDF
- 자료 PDF 위에 필기만 있는 경우
- 강의자료 PDF만 제공되는 경우

하지만 손글씨/필기 없이 `STT TXT + 강의자료 PDF`만 있는 경우가 별도 항목으로 없다. 이 때문에 STT를 primary_scope_anchor로 삼는 판단은 가능했지만, 명시 규칙이 없어 추론이 필요했다.

## 수정 대상

- `agent_rules/input/scope_anchor_rules.md`

## 요구사항

1. `STT TXT + 강의자료 PDF만 제공되는 경우` 섹션을 추가한다.
2. 다음 의미를 포함한다.
   - STT TXT를 본문 범위의 최종 기준(`primary_scope_anchor`)으로 삼는다.
   - 강의자료 PDF는 STT에서 실제로 설명된 용어, 공식, 슬라이드 순서, 예제 수치 확인용으로만 사용한다.
   - PDF에만 있고 STT에서 다루지 않은 내용은 본문에 추가하지 않고 PLAN 또는 최종 보고서에 `[자료 PDF 초과 범위]`로 기록한다.
   - STT에서 "다음 시간", "오늘은 하지 않음", "여기서 끝냄" 등으로 명시한 이후 범위는 PDF에 있어도 제외한다.
   - STT 품질이 낮아 문맥 연결이 불가능하면 범위 확정을 보류하고 `[TXT 누락 의심]`, `[대응 불확실]`, `[확인 필요]` 중 적절한 태그로 기록한다.
3. 기존 `손글씨 PDF + 음성 TXT + 강의자료 PDF` 규칙과 충돌하지 않도록 위치와 문장을 조정한다.

## 완료 기준

- 새 조합 규칙이 명시되어 있다.
- 강의자료 PDF가 본문 확장용이 아니라 보조 확인용임이 분명하다.
- STT가 명시적으로 제외한 이후 범위는 PDF에 있어도 제외한다는 기준이 포함되어 있다.
- 기존 primary_scope_anchor 원칙이 약화되지 않았다.

---

# Issue 2. H2 구분선 검증 기준의 non-empty line 판정 명시

## 배경

`열역학 8-2.md` 검증 중, 처음 작성한 구조 검증 스크립트가 각 H2 섹션의 마지막 줄이 정확히 `---`인지 검사했다. 실제 문서는 `---` 뒤에 빈 줄이 있고 다음 H2가 오는 정상 Markdown 형식이었지만, 초기 검증에서 모든 H2가 오류로 잘못 보고되었다.

## 수정 대상

- `agent_rules/schemas/validation_rules.json`
- 필요 시 `agent_rules/validation/structure_and_latex_validation.md`

## 요구사항

1. H2 구분선 판정 기준을 다음 의미로 명확히 한다.

```text
H2 구분선은 다음 H2 또는 문서 끝 이전의 마지막 non-empty line이 `---`이면 통과로 본다.
`---` 뒤의 빈 줄은 허용한다.
```

2. H2 제목 바로 다음 의미 줄이 `---`이면 안 된다는 기존 규칙은 유지한다.
3. JSON schema를 수정하는 경우 기존 필드 구조를 과도하게 바꾸지 말고, 설명 필드 또는 판정 보조 필드로 최소 변경한다.

## 완료 기준

- `---` 뒤 빈 줄을 허용한다는 기준이 명시되어 있다.
- H2 바로 다음 줄 구분선 금지 규칙은 유지되어 있다.
- `validation_rules.json` 수정 시 JSON 파서 검증이 통과한다.

---

# Issue 3. untracked 산출물 검증 절차 추가

## 배경

`열역학 8-2.md`와 `열역학_8-2_PLAN.md`는 새 파일이어서 git 상태상 untracked였다. 이 경우 `git diff --stat`와 `git diff -- <path>`에는 내용이 나타나지 않아, 별도로 `Get-Item` 또는 UTF-8 read 기반 확인이 필요했다.

## 수정 대상

- `agent_rules/execution/git_and_file_changes.md`
- 필요 시 `agent_rules/execution/json_and_reporting.md`
- 필요 시 `agent_rules/schemas/codex_policy.json`

## 요구사항

1. after-work git check 절차에 untracked 산출물 확인 기준을 추가한다.
2. 다음 의미를 포함한다.

```text
Untracked 산출물은 git diff 대상이 아니므로, 새로 생성한 파일은 `git status --short`로 목록을 확인한 뒤 `Get-Item` 또는 UTF-8 read 기반으로 존재 여부와 크기/핵심 구조를 별도 확인한다.
```

3. 기존 modified/deleted/staged 파일 구분 원칙은 유지한다.
4. untracked 파일을 임의로 stage하거나 삭제하지 않는다는 보호 원칙을 유지한다.

## 완료 기준

- untracked 산출물은 `git diff`에 나오지 않는다는 설명이 추가되어 있다.
- 새 산출물 존재 확인 절차가 명시되어 있다.
- 기존 변경 보호 원칙이 약화되지 않았다.

---

# Issue 4. Windows PDF 텍스트 추출 도구 후보 경로 안내 추가

## 배경

작업 중 Python PDF 라이브러리(`fitz`, `pypdf`, `PyPDF2`)는 없고 `pdftotext`도 PATH에서 발견되지 않았다. 하지만 실제로는 다음 경로에 `pdftotext.exe`가 있었다.

```text
C:\Program Files\Git\mingw64\bin\pdftotext.exe
```

직접 경로를 호출하자 슬라이드 PDF 텍스트 레이어를 정상 추출할 수 있었다.

## 수정 대상

- `agent_rules/execution/python_and_pdf_rendering.md`
- 또는 `agent_rules/input/pdf_reading_strategy.md`

## 요구사항

1. Windows에서 `pdftotext`가 PATH에 없을 때 후보 경로를 1회 확인할 수 있다는 안내를 추가한다.
2. 예시는 다음 경로를 포함할 수 있다.

```text
C:\Program Files\Git\mingw64\bin\pdftotext.exe
C:\Program Files\GIGABYTE\StableDiffusion\Data\git\mingw64\bin\pdftotext.exe
```

3. 특정 사용자 환경 경로를 필수 규칙으로 고정하지 말고 "후보 예시" 또는 "발견될 수 있음"으로 표현한다.
4. 도구가 없으면 설치하지 말고 기존 규칙에 따라 렌더링/OCR fallback으로 전환한다는 원칙을 유지한다.

## 완료 기준

- PATH에 없는 `pdftotext.exe` 후보 확인 절차가 추가되어 있다.
- 패키지 설치나 환경 변경을 요구하지 않는다.
- PDF fallback 원칙이 유지되어 있다.

---

# Issue 5. PowerShell 병렬 실행 실패 환경 이슈 예시 추가

## 배경

작업 초기에 여러 규칙 파일을 병렬로 읽는 과정에서 일부 PowerShell 호출이 다음 오류로 실패했다.

```text
windows sandbox: CreateProcessWithLogonW failed: 1056
```

같은 명령을 개별 또는 더 작은 묶음으로 재시도하면 정상 처리되었다.

## 수정 대상

- `agent_rules/execution/command_safety.md`
- 필요 시 `agent_rules/execution/json_and_reporting.md`

## 요구사항

1. Windows 샌드박스 환경에서 병렬 PowerShell 호출이 일시적으로 실패할 수 있음을 환경 이슈 예시로 추가한다.
2. 실패 시 같은 명령을 무한 재시도하지 말고, 작은 묶음 또는 개별 명령으로 재시도하라는 절차를 추가한다.
3. 이 실패가 파일 내용 오류나 인코딩 오류로 단정되지 않도록 주의 문구를 넣는다.
4. 최종 보고에서 환경 이슈로 기록할 수 있음을 명시한다.

## 완료 기준

- `CreateProcessWithLogonW failed: 1056` 또는 동등한 병렬 PowerShell 실행 실패 예시가 기록되어 있다.
- 재시도 방식이 제한적이고 안전하게 제시되어 있다.
- 위험 명령 제한 정책이 약화되지 않았다.

---

# Issue 6. 한글 경로 git 상태 보조 명령 안내 추가

## 배경

`git status --short`에서 한글 경로가 다음처럼 octal escape 형태로 표시되어 사람이 읽기 어려웠다.

```text
?? "output/\354\227\264\354\227\255\355\225\231/..."
```

최종 보고에는 사람이 읽을 수 있는 경로가 필요하므로 보조 확인 명령이 있으면 안전하다.

## 수정 대상

- `agent_rules/execution/git_and_file_changes.md`
- 필요 시 `agent_rules/execution/command_safety.md`

## 요구사항

1. `git status --short`를 canonical source로 유지한다.
2. 한글 경로 확인이 어려울 때 최종 보고용 보조 확인으로 다음 명령을 사용할 수 있음을 추가한다.

```text
git -c core.quotepath=false status --short
```

3. 보조 명령은 상태 판독 가독성을 위한 것이며, 파일 변경/복구/삭제 동작이 아님을 명시한다.
4. 한글 경로를 최종 보고에 옮길 때는 실제 파일 경로를 UTF-8로 확인하라는 기준을 추가한다.

## 완료 기준

- `core.quotepath=false` 보조 명령이 명시되어 있다.
- 기존 `git status --short` 기준이 대체되지 않고 유지되어 있다.
- 한글 경로 최종 보고 오류를 줄이는 확인 기준이 포함되어 있다.

---

# 최종 보고 요구사항

각 Issue 처리 후 최종 보고에는 다음 항목을 포함한다.

```text
## 작업 요약
### 처리한 Issue
- Issue 번호와 제목

### 수정한 파일
- 파일 경로 목록

### 변경 내용
- 핵심 변경 사항 요약

### 검증 결과
- git status --short 확인
- JSON 수정 시 JSON 파서 검증 결과
- 안전 정책 약화 여부 확인

### 잔여 이슈
- 남은 판단 불확실성 또는 후속 Issue
```
