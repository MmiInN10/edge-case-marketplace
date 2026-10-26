# edge-case-hunter 보고서

## 1. 개요

- 명칭: `edge-case-hunter` (엣지 케이스 발굴 에이전트)
- 형태: Claude Code 서브에이전트 플러그인
- 배포: GitHub 마켓플레이스 `MmiInN10/edge-case-marketplace`
- 버전: 1.0.0

## 2. 목적

- 코드를 읽고 터질 만한 입력·상황(엣지 케이스) 후보를 찾아 보고함
- 테스트 케이스를 정하기 전, 새 함수·모듈 작성 직후에 사용함
- 수정은 하지 않음. 후보 목록 제공까지가 역할임

## 3. 동작 방식

| 항목 | 내용 |
|------|------|
| 도구 | `Read`, `Grep`, `Glob` (읽기 전용) |
| 코드 실행 | 하지 않음 |
| 파일 수정·생성 | 하지 않음 |
| 근거 | 모든 후보에 `파일:줄` 표기. 줄을 못 대면 후보에서 제외 |
| 구분 | 확인됨(코드상 깨짐이 따라 나옴) / 추측(호출부·환경을 몰라 가능성만 있음) |
| 최대 건수 | 10건. 영향·발생 가능성 순으로 정렬 |

점검 분류: 경계값, 빈 값, 타입, 숫자, 문자열, 파일·경로, 크기, 외부 호출, 상태·순서, 시간

## 4. 설치

```
/plugin marketplace add MmiInN10/edge-case-marketplace
/plugin install edge-case-hunter@edge-case-marketplace
/reload-plugins
```

- `/reload-plugins` 결과의 agents 수가 1 늘면 설치 완료
- 레포가 Private이면 설치자 계정에 접근 권한이 필요함

## 5. 사용법

```
@edge-case-hunter:edge-case-hunter train.py 의 load_dataset 함수 엣지 케이스 찾아줘
```

- 말로 요청해도 호출됨: "엣지 케이스 찾아줘", "이 코드 어떤 경우에 깨져?", "경계값 확인해줘"
- 수정은 별도 요청으로 진행함: "1번부터 고쳐줘"

## 6. 출력 형식

아래는 형식 예시이며 실제 실행 결과가 아님.

```
실행하지 않고 코드만 읽은 결과입니다.
대상: train.py / load_dataset

| # | 위치 | 입력·상황 | 예상 증상 | 구분 |
|---|------|-----------|-----------|------|
| 1 | `train.py:42` | 빈 폴더 `""` | 빈 리스트 인덱싱 IndexError | 확인됨 |

[테스트로 옮기기]
- 먼저 테스트할 3개와 기대 결과

[확인이 필요한 것]
- 코드만으로 판단하지 못한 것
```

## 7. 제한 사항

- 코드를 실행하지 않으므로 보고 내용은 실제 재현으로 확인된 것이 아님
- 호출부·라이브러리 버전·환경에 따라 달라지는 항목은 `추측`으로 표시됨
- 스타일·네이밍·리팩토링 지적은 범위 밖임

## 8. 검증 현황

| 항목 | 상태 |
|------|------|
| JSON 구조 검증 (`claude plugin validate`) | 통과 |
| 동일 지침으로 대체 에이전트 실행 (`v11_02_streamlit_advan.py`) | 완료, 후보 10건 보고 |
| GitHub 마켓플레이스 설치 후 플러그인 직접 호출 | 미확인 |

## 9. 구조

```
.claude-plugin/marketplace.json
plugins/edge-case-hunter/
├─ .claude-plugin/plugin.json
└─ agents/edge-case-hunter.md
```
