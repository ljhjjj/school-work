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
