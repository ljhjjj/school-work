# 열역학 8-1 PLAN

## 작업 범위
- 과목: 열역학
- 차시: 8-1
- 입력 폴더: `inbox/열역학/8-1`
- 주 범위 기준(primary_scope_anchor): `handwritten_note.pdf`
- 보조 자료: `lecture_slide.pdf`

## 입력 판정
- [x] `handwritten_note.pdf` 3쪽을 렌더링하여 판독함.
- [x] `lecture_slide.pdf` 18쪽을 렌더링하여 강의명, 개요, 용어를 보조 확인함.
- [x] 슬라이드의 `Irreversibility`, `Reversible cycle`, `Carnot cycle` 등 필기 PDF에 본문 설명이 없는 항목은 본문 확장 범위에서 제외함.

## 작성 계획
- [x] 열역학 제2법칙의 필요성과 자연 과정의 방향성을 정리함.
- [x] 열효율과 냉장고/열펌프 성능계수(COP)를 정리함.
- [x] 열저장조, Kelvin-Planck statement, Clausius statement를 정리함.
- [x] Kelvin-Planck statement와 Clausius statement의 동등성 논증을 정리함.
- [x] 필기 3쪽의 `Reversed/Reversible process` 내용을 `## 7. 가역 과정의 기본 의미`로 보완함.
- [x] 최종 마크다운 파일을 `output/열역학/열역학 8-1.md`로 저장함.
- [x] 검증 기준에 따라 H1/H2/H3 구조, 수식 블록, 경로를 확인함.

## 페이지 판독 기록
- `handwritten_note.pdf` p.1: 열역학 제2법칙의 필요성, 열효율, COP, 열저장조 개념 확인
- `handwritten_note.pdf` p.2: Kelvin-Planck statement, Clausius statement, Clausius 위배에서 Kelvin-Planck 위배로 이어지는 논증 확인
- `handwritten_note.pdf` p.3: Kelvin-Planck 위배에서 Clausius 위배로 이어지는 논증과 `Reversed/Reversible process` 내용 확인
- `lecture_slide.pdf`: 강의명, 개요, 영문 용어 보조 확인. 필기 PDF 범위를 넘는 가역 사이클/카르노 사이클 내용은 본문 확장에 사용하지 않음

## 입력 커버리지 검증
- [x] `handwritten_note.pdf` p.1 핵심 항목 반영
- [x] `handwritten_note.pdf` p.2 도식 논증 반영
- [x] `handwritten_note.pdf` p.3 도식 논증 및 `Reversed/Reversible process` 내용 반영
- [x] `lecture_slide.pdf`는 용어·개요 보조 확인으로만 사용
- [x] 구조 검증 결과와 입력 커버리지 검증 결과를 분리해 기록함

## 범위 기록
- [필기 우선] 본문은 `handwritten_note.pdf`의 3쪽 내용에 근거함.
- [보정] `lecture_slide.pdf`는 `2nd law of thermodynamics - 2` 강의 개요와 영문 용어 확인에만 사용함.
- [자료 PDF 초과 범위] 슬라이드 개요의 비가역성, 가역 사이클, 카르노 사이클은 필기 PDF에 구체 설명이 없어 본문에 전개하지 않음.

## 검증 로그
- `Get-Content -LiteralPath agent_rules\schemas\validation_rules.json -Encoding UTF8`
- `git status --short`
- `git diff --stat`
- 수동 검증: H1 1개, 번호형 H2, 번호형 H3, 독립 블록 수식, 잔여 이슈 태그 확인

## 잔여 이슈
- 치명적 잔여 이슈: 없음.
- 비치명적 판독/보정 이슈: 필기 3쪽의 `Reversed process`는 문맥상 `reversible process`로 보아 가역 과정으로 보정했고, `## 7. 가역 과정의 기본 의미`에 반영 완료함.
- 비치명적 우회 처리: PDF 텍스트 직접 추출은 불충분하여 페이지 이미지 렌더링 후 판독함.
- 환경 이슈: PowerShell profile 접근 권한/인코딩 경고가 로그에 섞였으나 산출물 생성과 최종 노트 품질에는 영향 없음.
