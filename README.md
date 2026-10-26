# edge-case-marketplace

Claude Code용 엣지 케이스 발굴 에이전트 `edge-case-hunter` 를 담은 마켓플레이스입니다.

## edge-case-hunter

코드를 읽고 경계값·빈 입력·예상 밖 입력 등 터질 만한 상황을 `파일:줄` 근거와 함께 우선순위 표로 보고합니다.
읽기 전용(`Read`, `Grep`, `Glob`)이라 코드를 고치거나 실행하지 않습니다.

## 설치

Claude Code 프롬프트에 한 줄씩 입력합니다.

```
/plugin marketplace add MmiInN10/edge-case-marketplace
/plugin install edge-case-hunter@edge-case-marketplace
/reload-plugins
```

## 사용

```
@edge-case-hunter:edge-case-hunter train.py 의 load_dataset 함수 엣지 케이스 찾아줘
```

"엣지 케이스 찾아줘", "이 코드 어떤 경우에 깨져?", "경계값 확인해줘" 처럼 말로 요청해도 Claude 가 알아서 부릅니다.

## 보고 형식

아래는 형식 예시이며 실제 실행 결과가 아닙니다.

```
실행하지 않고 코드만 읽은 결과입니다.
대상: train.py / load_dataset

| # | 위치 | 입력·상황 | 예상 증상 | 구분 |
|---|------|-----------|-----------|------|
| 1 | `train.py:42` | 빈 폴더 `""` | 빈 리스트 인덱싱 IndexError | 확인됨 |

[테스트로 옮기기]
- <먼저 테스트할 3개와 기대 결과>

[확인이 필요한 것]
- <코드만으로 판단하지 못한 것>
```

- **확인됨**: 코드를 읽어 그 입력에서 깨짐이 따라 나오는 경우
- **추측**: 호출부나 환경을 몰라 가능성만 말하는 경우

## 하지 않는 일

- 코드 수정, 파일 생성, 코드 실행
- 근거를 못 찾은 후보 만들기
- 스타일·네이밍·리팩토링 지적

## 구조

```
.claude-plugin/marketplace.json
plugins/edge-case-hunter/
├─ .claude-plugin/plugin.json
└─ agents/edge-case-hunter.md
```
