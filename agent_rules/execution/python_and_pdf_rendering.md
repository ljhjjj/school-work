# Python and PDF Rendering

## [P0/P1] 5. 명령 실행 원칙

### [P1] 5.5 Python 실행 환경 고정
- 기본 검증 명령은 `conda run -n pdfkit310 python --version`을 우선 사용한다.
- `conda` 환경이 없거나 실패하면 `python --version`을 시도한다.
- `python`도 실패하면 `py --version`을 시도한다.
- `py --version` 실패는 Python 런처 환경 이슈로 기록하되, 이를 이유로 PDF/OCR 작업을 즉시 중단하지 않는다.
- 실제 작업에 사용할 Python 실행 명령은 성공한 검증 결과를 기준으로 선택한다.

### [P1] 5.6 PDF 페이지 렌더링 방식 우선순위
- PDF 판독용 이미지는 각 페이지를 독립 이미지로 렌더링하는 방식을 기본으로 한다.
- 렌더링 우선순위는 다음 순서를 따른다.
  1. Poppler 또는 MuPDF 기반 페이지별 렌더링
  2. Windows.Data.Pdf 기반 페이지별 렌더링
  3. Edge screenshot은 최후의 임시 확인용으로만 사용
- Edge headless screenshot은 저장 권한, viewport 캡처, 다중 페이지 처리 한계가 있으므로 기본 판독 경로로 사용하지 않는다.
- 렌더링 결과 파일명은 `{source_basename}_page_{page_number}.png` 형식을 기본으로 한다.
  - 예: `handwritten_note_page_001.png`, `lecture_slide_page_012.png`
- 렌더링 실패 시 권한 문제, 경로 문제, 도구 문제, 입력 PDF 문제를 구분해 PLAN 또는 최종 보고서의 `[환경 이슈]`나 `[잔여 이슈]`에 기록한다.
- 렌더링 도구 실패가 있어도 다른 페이지별 렌더링 방식으로 전환 가능하면 작업 실패로 판정하지 않는다.

### [P2] 5.7 렌더링 이미지 캐시 보관 모드
- 기본 모드: PDF 판독용 렌더링 이미지를 작업 완료 후 즉시 삭제하지 않고 24시간 보관 대상으로 둔다.
- 디버그 모드: 렌더링 이미지를 `output/_render_cache/{subject}/{lecture_or_unit}/` 아래에 보관한다.
- 사용자가 “캐시를 지워줘”라고 요청하면 기본 모드와 디버그 모드의 관련 캐시를 삭제할 수 있다.
- 캐시 보관은 원본 PDF나 최종 Markdown 산출물의 보관 정책을 변경하지 않는다.
