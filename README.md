# Chunk Module UI PoC

[![Chunk Module Preview](assets/upstage-deck-03.png)](https://graceful-semolina-d6a50d.netlify.app/)

> `Parse → Chunk → Extract` 흐름에서, Chunk 패널 UX를 검증하기 위한 정적 목업 PoC입니다.

🔗 **Live Demo**: [https://graceful-semolina-d6a50d.netlify.app/](https://graceful-semolina-d6a50d.netlify.app/)

---

## 이 프로젝트의 현재 범위

이 저장소는 **UI/상호작용 PoC**에 집중합니다.

- `chunk_json.html` 단일 파일 기반의 정적 목업
- `chunk_poc_fixed_example.json` 고정 예시 결과를 화면/다운로드에 사용
- 파일 업로드, 토글 전환, 복사/다운로드 같은 사용자 플로우 검증

다음 항목은 현재 구현되어 있지 않습니다.

- 실제 Parse 단계와의 자동 연동
- 서버 호출 기반 LLM 추론/스키마 생성
- 실제 문서별 동적 변환 결과 생성

---

## 왜 이 PoC가 필요한가

복잡한 문서(시험지, 규정, 표 포함 문서)는 Extract 입력 스키마 설계 난이도가 높습니다.  
이 PoC는 “정확한 변환 엔진”보다 먼저, 다음 제품 가설을 검증하기 위한 단계입니다.

- 비전문가도 Chunk 패널에서 입력/스키마/결과 흐름을 이해할 수 있는가
- 사용자 의도 입력 UI가 스키마 설계 UX에 도움을 주는가
- Auto/User 모드 전환과 업로드 경험이 협업 시나리오에 충분히 직관적인가

---

## 실제 구현 기능

### 1) 입력 방식 전환 (Auto / 직접 업로드)
- `밀가루(raw)`와 `붕어빵 틀(mold)` 각각 Auto/User 토글 제공
- User 모드에서 JSON 파일 선택 및 Drag & Drop 업로드 지원
- JSON 파싱 실패/형식 오류 메시지 표시

### 2) 의도 입력 영역
- 자유 텍스트 입력창 제공 (placeholder 포함)
- 현재는 UX 검증용 입력이며, 실행 결과 계산에는 직접 반영되지 않음

### 3) 붕어빵 틀 Auto 하위 모드
- `저장된 설계(stored_schema)` / `스키마 자동 설계(llm_design)` UI 토글 제공
- 현재는 문구/상태 반영 중심이며, 실제 서버 설계 호출은 없음

### 4) 변환 실행 버튼
- 실행 시 로딩/성공 상태 전환
- 결과 영역에 고정 예시 JSON 렌더링
- 결과 복사/다운로드 제공

---

## CTO 관점 검토 포인트

### 강점
- **하드코딩 최소화 방향성**: 문구/상태 키를 상수 객체(`CHUNK_UI_COPY`, `CHUNK_UI_TIMING` 등)로 분리
- **일관성**: Auto/User 토글, 업로드, 결과 패널이 공통 패턴으로 구성
- **협업 용이성**: PoC 고정 예시 JSON을 별도 파일과 DOM 주석으로 동기화 가이드 제공
- **확장 준비도**: `moldAutoMode`, `runKind`, 상태 객체 등 런타임 구조가 API 연동 확장을 고려한 형태

### 현재 한계
- **실행 결과가 고정값**: 입력 조합과 무관하게 동일 JSON 출력
- **백엔드 계약 미연동**: LLM/스키마 설계는 UI 상태만 존재
- **검증 지표 부재**: 정확도/소요시간/에러율 등 정량 지표 수집 코드 없음

---

## 파일 구조

- `chunk_json.html`: PoC UI 본체 (스타일 + 스크립트 포함)
- `chunk_poc_fixed_example.json`: PoC 고정 결과 예시 JSON
- `assets/upstage-deck-03.png`: 발표용 이미지 에셋

---

## 실행 방법

정적 HTML이므로 별도 빌드 도구가 필요 없습니다.

```bash
# 저장소 루트에서
open chunk_json.html
```

또는 브라우저에서 `chunk_json.html` 파일을 직접 열어 확인할 수 있습니다.

---

## 운영/확장 시 권장 사항

- 결과 생성 로직을 UI에서 분리해 API 계층으로 이전
- 입력 스키마/출력 스키마 계약(JSON Schema) 버전 관리 도입
- 실행 결과에 trace id, latency, 실패 사유를 남기는 관측성(observability) 추가
- 고정 예시 기반 PoC에서 벗어나 회귀 테스트 가능한 fixtures 세트 구성

---

## 참고

현재 문서는 **“지금 코드가 실제로 하는 일”** 기준으로 작성되었습니다.  
실서비스 연동(실제 LLM 설계/변환 API)이 붙는 시점에, README를 `PoC 모드`와 `연동 모드`로 분리해 관리하는 것을 권장합니다.
