# AI Problem Review

> 대상 작업: `inbox/열역학/8-2` 강의노트 생성
> 작성일: 2026-05-04
> 목적: 실제 작업 중 발생한 실행/규칙/검증 문제를 기록하고, 향후 `agent_rules/` 개선 항목으로 전환할 수 있게 정리한다.

---

## 1. 병렬 파일 읽기 중 PowerShell 샌드박스 프로세스 오류

### 발생 상황

작업 초기에 `agent_rules/`의 여러 가이드 파일을 `multi_tool_use.parallel`로 동시에 읽는 과정에서 일부 `shell_command`가 다음 오류로 실패했다.

```text
windows sandbox: CreateProcessWithLogonW failed: 1056
```

같은 파일을 개별 또는 더 작은 묶음으로 다시 읽었을 때는 정상 처리되었다.

### 영향

- 규칙 파일 확인이 한 번에 끝나지 않고 재시도가 필요했다.
- 실패 원인이 파일 인코딩, 경로, 권한, 병렬 실행 제한 중 무엇인지 즉시 구분하기 어려웠다.
- 작업 자체는 지연만 있었고 산출물 품질에는 직접 영향이 없었다.

### 개선 제안

- `command_safety` 또는 실행 가이드에 Windows 샌드박스에서 병렬 PowerShell 호출이 실패할 수 있음을 환경 이슈 예시로 추가한다.
- 규칙 파일 다중 읽기 시 권장 병렬 개수를 제한하거나, 실패 시 "작은 묶음으로 재시도" 절차를 명시한다.
- 최종 보고의 환경 이슈 예시에 `CreateProcessWithLogonW failed: 1056`을 추가한다.

---

## 2. `STT TXT + 강의자료 PDF` 조합의 primary scope anchor 규칙이 명시적으로 부족함

### 발생 상황

이번 입력은 다음 조합이었다.

- `lecture_stt_20260503_153054.txt`
- `lecture_slide(1).pdf`
- `lecture_slide(2).pdf`

현재 `scope_anchor_rules.md`에는 `손글씨 PDF + 음성 TXT + 강의자료 PDF`, `필기 PDF + 강의자료 PDF`, `자료 PDF 위 필기`, `강의자료 PDF만` 조합은 명시되어 있지만, 손글씨/필기 없이 `STT TXT + 강의자료 PDF`만 있는 경우가 별도 항목으로 분리되어 있지 않았다.

### 영향

- STT를 primary anchor로 삼는 판단은 기존 원칙과 일관되지만, 명시 규칙이 없어 추론이 필요했다.
- 슬라이드 PDF에 STT보다 더 넓은 내용이 있을 때 어디까지 제외해야 하는지 작업자가 기존 유사 규칙을 적용해야 했다.
- 이번 작업에서는 `lecture_slide(2).pdf`의 Entropy 이후 내용을 STT가 다음 시간으로 넘겼기 때문에 제외했지만, 이 판단은 별도 조합 규칙이 있으면 더 안전하다.

### 개선 제안

`scope_anchor_rules.md`에 다음 조합을 추가한다.

```text
STT TXT + 강의자료 PDF만 제공되는 경우
- STT TXT를 본문 범위의 최종 기준(primary_scope_anchor)으로 삼는다.
- 강의자료 PDF는 STT에서 실제로 설명된 용어, 공식, 슬라이드 순서, 예제 수치 확인용으로만 사용한다.
- PDF에만 있고 STT에서 다루지 않은 내용은 본문에 추가하지 않고 [자료 PDF 초과 범위]로 기록한다.
- STT에서 "다음 시간", "오늘은 하지 않음", "여기서 끝냄" 등으로 명시한 이후 범위는 PDF에 있어도 제외한다.
```

---

## 3. PDF 텍스트 추출 도구 발견 절차가 규칙에 충분히 구체화되어 있지 않음

### 발생 상황

Python PDF 라이브러리와 일반 PATH의 `pdftotext`를 확인했을 때 다음 상태였다.

- `fitz`, `pypdf`, `PyPDF2`: 미설치
- `pdftotext`: PATH에서는 발견되지 않음
- 실제 사용 가능 파일: `C:\Program Files\Git\mingw64\bin\pdftotext.exe`

결국 `Get-ChildItem`으로 `pdftotext.exe`를 찾아 직접 경로 호출했다.

### 영향

- PDF 판독 도구가 "없다"고 잘못 판단할 가능성이 있었다.
- `lecture_slide(1).pdf`, `lecture_slide(2).pdf`는 텍스트 레이어가 충분했으므로 렌더링/OCR 없이 빠르게 처리할 수 있었지만, 도구 발견이 늦어졌다.

### 개선 제안

- `python_and_pdf_rendering.md` 또는 `pdf_reading_strategy.md`에 Windows에서 확인할 후보 경로를 추가한다.
- 예시:

```text
Windows에서 `pdftotext`가 PATH에 없으면 다음 후보를 1회 확인한다.
- C:\Program Files\Git\mingw64\bin\pdftotext.exe
- C:\Program Files\GIGABYTE\StableDiffusion\Data\git\mingw64\bin\pdftotext.exe
```

- 단, 특정 사용자 환경 경로는 일반 규칙으로 과도하게 고정하지 말고 "후보 예시"로 둔다.

---

## 4. H2 구분선 검증 스크립트가 빈 줄 처리 때문에 false positive를 냄

### 발생 상황

처음 작성한 구조 검증 스크립트가 각 H2 섹션의 마지막 줄이 정확히 `---`인지 검사했다. 그러나 실제 Markdown은 `---` 뒤에 빈 줄이 있고 다음 H2가 오는 정상 형식이었다.

초기 검증 결과는 모든 H2 섹션을 구분선 오류로 잘못 보고했다. 이후 빈 줄을 제외한 마지막 의미 줄이 `---`인지 검사하도록 수정하자 통과했다.

### 영향

- 정상 문서가 구조 오류처럼 보이는 false positive가 발생했다.
- 자동 수정 정책과 연결될 경우 불필요한 재수정이 일어날 수 있다.

### 개선 제안

`validation_rules.json` 또는 검증 가이드에 H2 구분선 판정 기준을 더 명확히 적는다.

```text
H2 구분선은 다음 H2 또는 문서 끝 이전의 마지막 non-empty line이 `---`이면 통과로 본다.
`---` 뒤의 빈 줄은 허용한다.
```

---

## 5. `git status --short`의 한글 경로 표시가 사람이 읽기 어렵게 나옴

### 발생 상황

`git status --short`에서 한글 경로가 다음처럼 octal escape 형태로 표시되었다.

```text
?? "output/\354\227\264\354\227\255\355\225\231/..."
```

작업자는 실제 생성 파일 경로를 알고 있었기 때문에 최종 보고에서는 사람이 읽을 수 있는 경로로 정리했지만, 상태 출력만으로는 빠르게 확인하기 어려웠다.

### 영향

- untracked 파일이 많을 때 신규 파일과 기존 untracked 파일을 구분하기 어렵다.
- 최종 보고에서 한글 경로를 잘못 옮길 위험이 있다.

### 개선 제안

- Windows/한글 경로 환경에서는 필요 시 다음 보조 명령을 허용 예시로 둔다.

```text
git -c core.quotepath=false status --short
```

- 단, canonical source는 여전히 `git status --short`로 두되, 사람이 읽는 최종 보고용 보조 확인으로 명시한다.

---

## 6. `git diff --stat`만으로는 untracked 산출물 요약이 비어 보임

### 발생 상황

이번 작업에서 생성된 `output/열역학/열역학 8-2.md`, `output/열역학/열역학_8-2_PLAN.md`는 untracked 파일이었다. 따라서 `git diff --stat`와 `git diff -- <path>`에는 내용이 나타나지 않았다.

### 영향

- 후속 검증에서 변경량이 없는 것처럼 보일 수 있다.
- 최종 보고에서 untracked 산출물은 별도로 `Get-Item` 또는 파일 읽기 기반으로 확인해야 했다.

### 개선 제안

after-work git check 절차에 다음 보조 확인을 추가한다.

```text
Untracked 산출물이 있으면 git diff 대상이 아니므로 `Get-Item` 또는 UTF-8 read 기반으로 존재 여부와 크기를 별도 확인한다.
```

---

## 우선순위 제안

1. **P0/P1:** `STT TXT + 강의자료 PDF` scope anchor 규칙 추가
2. **P1:** H2 구분선 검증 기준의 non-empty line 판정 명시
3. **P1:** untracked 산출물 검증 절차 추가
4. **P2:** Windows PDF 도구 후보 경로 안내
5. **P2:** PowerShell 병렬 실행 실패 환경 이슈 예시 추가
6. **P2:** 한글 경로 git 상태 보조 명령 안내

---

## 이번 작업의 결론

이번 `8-2` 작업은 완료되었고 산출물 자체의 치명적 이슈는 없었다. 다만 규칙 관점에서는 `STT + 슬라이드 PDF` 조합이 명시되어 있지 않아 scope anchor 판단을 추론해야 했고, 검증/보고 단계에서는 Windows 환경 특유의 도구 발견, 한글 경로 표시, untracked 파일 확인 문제가 반복될 여지가 확인되었다.
