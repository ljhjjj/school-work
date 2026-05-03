# Note_creation_guide

## Output Language Policy
- Final markdown lecture notes must be written in Korean by default.
- Use the objective Korean written style `~한다`, `~이다`.
- Technical terms should be written in Korean first, with English terms in parentheses when useful.
  - Examples: `전단응력(Shear Stress)`, `포아송 비(Poisson's ratio)`
- File names, path examples, tags, and source titles may preserve Korean or mixed notation as needed.
- Do not write the final note in English merely because the AI guidelines or PLAN are written in English.
- Write the final markdown lecture note in English only when the user explicitly requests English output, such as "영어로 출력" or "영문 노트로 작성".

## 1. 파일 네이밍 및 저장 경로 규칙
- **파일명 규칙:** `[과목명] [주차]-[차시].md`  
  예: `고체역학 3-1.md`
- **저장 경로 규칙:** 현재 프로젝트 루트(workspace) 하위의 `output/[과목명]/` 폴더 내 저장한다.
  - 예: `output/고체역학/고체역학 3-1.md`
- **PLAN 파일 저장 경로:** 본문 파일과 같은 `output/[과목명]/` 폴더에 저장한다.
  - 예: `output/고체역학/고체역학_3-1_PLAN.md`
- **[중요: Windows 호환성 및 인코딩]**
  - 한글 포함 파일/폴더명은 셸 명령 시 **따옴표(" ")**로 감쌉니다.
  - 모든 `.md` 파일은 **UTF-8 인코딩**으로 저장합니다.
- **[중요] 폴더 생성 의무화:** 
  - *PowerShell (권장):* `New-Item -ItemType Directory -Force -Path "output/[과목명]"`
  - *CMD:* `if not exist "output\[과목명]" mkdir "output\[과목명]"`

## 2. 본문 생성 규칙
- **디테일 보존 원칙:** 요약 요청이 없는 한 세부 내용, 예시, 도출 과정을 생략하지 마세요.
- **긴 문서의 자율 연속 작성:** 결과물이 방대해 출력이 끊기더라도 묻지 말고 PLAN 파일을 참조해 멈춘 부분부터 자율적으로 이어서 작성/저장합니다.
- **[실패 복구(Fallback) 절차]:** 툴 오류, 저장 실패, 토큰 한계 등으로 연속 진행이 불가능해진 경우:
  1. 현재까지 완료한 범위를 PLAN 파일에 정확히 기록(`[x]`)합니다.
  2. 실패 원인을 `_PLAN.md`의 `### ⚠️ 실행 이슈` 섹션에 남깁니다.
  3. 다음 재개 지점을 명확히 표시하고 대기합니다.

## 3. 문서 구조 및 서식 규칙
1. **메인 제목 (H1) 및 대주제 (H2)**
   - 양식: `# [과목명] [주차]-[차시]: [주제]`, `## 1. 주제명`
   - 각 H2 섹션의 끝에는 다음 H2로 넘어가기 전에 구분선 `---`를 배치한다.
   - H2 제목 바로 다음 줄에 `---`를 배치하지 않는다.
2. **[중요] 콘텐츠 유형별 포맷 분리**
   - **개념 정의 및 특징:** 불릿 포인트(`-`)를 사용하여 개조식 작성. (볼드체 강조)
   - **수식 도출 및 논리 전개:** 불릿 압축 금지. 번호(`1., 2.`)를 매긴 단계별 서술이나 상세한 줄글로 작성.
   - **예시 및 부연 설명:** 별도 예제 블록(`> **예시:**`)에 작성하여 보존.
3. **수식 (GitHub Flavored LaTeX)**
   - 인라인 수식: `$수식$`
   - **[중요: 블록 수식 렌더링 안전성]:** 렌더링 충돌 방지를 위해, 앞뒤 줄바꿈을 포함한 독립 블록으로 작성하세요.
     (올바른 예:
     $$
     \sigma = \frac{P}{A}
     $$
     )
4. **어투:** '~한다', '~이다' 형태의 간결하고 객관적인 문어체.

--- (마크다운 문서 내부 예시 포맷: 고체역학 3-1.md) ---

# 고체역학 3-1: 응력과 변형률

## 1. 응력(Stress)의 개념

물체에 외부 하중이 작용할 때, 이에 저항하기 위해 물체 내부에 발생하는 단위 면적당 내력을 의미한다.

### 1) 수직응력과 전단응력
- **수직응력 (Normal Stress):** 단면에 수직한 방향으로 작용하는 응력이다. 인장응력(+)과 압축응력(-)으로 구분되며 기호는 $\sigma$를 사용한다.
  $$
  \sigma = \frac{P}{A}
  $$

> **핵심:** 응력은 단순히 가해진 힘의 크기가 아니라 면적에 대한 비율이다.

---

## 2. 변형률(Strain)의 정의

외력에 의해 물체의 형태나 크기가 변하는 정도를 원형에 대한 비율로 나타낸 무차원 물리량이다.

### 1) 수직 변형률의 도출 과정
1. 단면적이 일정한 봉에 인장 하중이 작용한다고 가정한다.
2. 원래 길이($L$)에 대하여 하중 작용 후 늘어난 길이의 변화량을 $\delta$라고 정의한다.
3. 변형률 $\epsilon$은 다음과 같이 정의된다.
   $$
   \epsilon = \frac{\delta}{L}
   $$
