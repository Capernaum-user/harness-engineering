---
category: "AI-Governance"
major_category: "AI-Governance"
middle_category: "Harness"
minor_category: "Prompt"
category_path: "AI-Governance/Harness/Prompt"
tags:
  - "human/korean"
  - "harness"
  - "prompt"
  - "category/ai-governance"
  - "category/ai-governance/harness"
  - "category/ai-governance/harness/prompt"
created_at: "YYYY-MM-DD"
updated_at: "YYYY-MM-DD"
project: "harness"
type: "spec"
tool: "claude"
status: "active"
visibility: "internal"
source_path: ""
related:
  - "00_Harness/harness-engineering-index.md"
  - "00_Harness/harness-change-history.md"
  - "{{SHARED_HUB}}"
  - "{{BOOT_PROTOCOL}}"
  - "00_Harness/task-execution-protocol.md"
---
# 프롬프트 표준 (Prompt Standards)
# Harness Engineering v1

Tags: #human/korean #harness #prompt
관련 문서: [[00_Harness/harness-engineering-index.md]] · [[00_Harness/harness-change-history.md]] · {{SHARED_HUB}} · {{BOOT_PROTOCOL}} · [[00_Harness/task-execution-protocol.md]]

사용자가 AI에게 효과적으로 작업을 지시하는 방법과 AI가 프롬프트를 해석하는 기준을 정의한다.




## 1. 효과적인 프롬프트의 5가지 요소

```
1. 목표 (WHAT): 무엇을 원하는가?
2. 맥락 (CONTEXT): 왜, 어떤 상황에서?
3. 범위 (SCOPE): 어디까지, 어느 파일, 어느 부분?
4. 제약 (CONSTRAINTS): 하지 말아야 할 것, 유지해야 할 것?
5. 형식 (FORMAT): 결과물의 형태는?
```

---

## 2. 작업 유형별 프롬프트 템플릿

### 코드 작성

```
[목표] Python으로 [기능]을 구현해줘.
[맥락] [프로젝트명]의 [모듈명]에 추가할 예정이야.
[범위] [파일경로]의 [클래스/함수]를 수정하거나 새 파일로 만들어줘.
[제약] 기존 [인터페이스/API] 시그니처는 변경하지 마.
[형식] 완성된 실행 가능 코드로 줘.
```

### 코드 수정/버그 수정

```
[목표] [파일경로]:[라인번호]의 [버그 설명]을 수정해줘.
[맥락] [현상 설명 — 예: 어떤 입력에서 어떤 오류가 발생]
[범위] 해당 버그만 수정해. 다른 부분은 건드리지 마.
[제약] 기존 테스트가 깨지면 안 돼.
```

### Vault 문서 작성

```
[목표] [주제]에 대한 [type] 문서를 만들어줘.
[맥락] [프로젝트명] 프로젝트용이야. [배경 설명]
[범위] [저장 위치] 또는 01_projects/[project]/ 에 저장해줘.
[형식] 한국어로, 표준 frontmatter 포함해서.
```

### 리서치

```
[목표] [주제]에 대해 조사해줘.
[맥락] [프로젝트명]에서 [어떤 결정]을 위해 필요해.
[범위] 최신 정보 위주로, 출처 명시해줘.
[형식] TL;DR 먼저, 이후 상세 내용. Vault 문서로 저장해줘.
```

### 인수인계 요청

**동일 도구 세션 간 위임 (1차 경로) — 핸드오프 릴레이(ORDER/REPORT 파일 교환):**
```
이 작업을 자족적 ORDER로 만들어 핸드오프 릴레이에 넘겨줘.
```
워커 세션은 릴레이에서 대기 중인 ORDER를 클레임해 수행하고 REPORT를 돌려준다. 상세: [[00_Harness/agent-handoff-protocol.md]].

**타 도구 인수인계·세션 연속 기록 (릴레이 비대상):**
```
지금까지 작업한 내용을 [Gemini/Codex/다음 세션]에 인수인계할 수 있게
work diary에 기록해줘. 재개 지점도 명확히 써줘.
```

---

## 3. 시각적 보고 표준 (Visual-First Reporting)

매체 선택은 산출물 유형에 따라 갈린다 — 하나의 형식을 기본값으로 강제하지 않는다:

1. **문서 내 다이어그램** (아키텍처, 흐름도, 의사결정 트리): **Mermaid 코드펜스 우선** — 가장 안정적이고 정적 사이트(예: Quartz)가 브라우저에서 직접 렌더한다. 라벨 줄바꿈은 `<br/>`를 쓰고 `\n`은 금지.
2. **일반 "시각화" 요청의 기본 매체**: **공개 발행 사이트 {{PUBLISH_SITE}} 노트 발행** (예: Quartz 같은 정적 사이트). 상세: {{SHARED_HUB}}의 Visualization Publishing Policy.
3. **명시적 standalone HTML 요청**: 문서 디자인 킷 라우터({{DOCUMENT_LANE_ROUTER}}) 규격으로 {{HTML_DOCS_DIR}} 에 생성 + 색인 빌더 실행.
4. **손그림 다이어그램**: Excalidraw 캔버스(`localhost:3000`, `mcp__excalidraw__*` 도구 또는 캔버스 API)에서 조작한 뒤 저장한다. **`.excalidraw.md` 내부 JSON을 AI가 직접 수정하지 않는다** — 캔버스를 거치지 않은 수기 JSON 편집은 드로잉을 깨뜨릴 수 있다.

중요한 노드에는 Obsidian 내부 링크(`[[...]]`)를 포함해 세부 문서로의 연결성을 확보한다.

---


## 4. AI 응답 원칙 (모든 AI 공통)

```
1. 감정 표현, 공감 표현, "좋은 질문입니다" 류 preamble 제거
2. 답변 서두에 TL;DR (결론) 먼저
3. 불확실한 내용은 "확인 필요"로 명시, 추측 단정 금지
4. "~하시면 될 것 같습니다" 완곡 표현 → 단정형으로
5. "도움이 되었길 바랍니다" 마무리 인사 제거
6. 완료하지 않은 항목을 완료했다고 보고하지 않음
```

---

## 5. 컨텍스트 윈도우 효율 관리

긴 세션에서 컨텍스트 낭비를 줄이는 방법:

```
✅ 핵심 파일만 읽기 (관련 없는 파일 읽기 자제)
✅ 큰 파일은 필요한 부분만 읽기 (offset, limit 활용)
✅ 반복 검색 대신 첫 검색 결과를 충분히 활용
✅ 이미 읽은 파일 재읽기 자제
✅ 완료된 항목은 메모리/work diary로 외부화
```

---

## 6. 작업 지시 우선순위 해석

이 순서는 사용자 지시 **간** 해석 순서일 뿐이다. 보안·데이터 무결성 관련 충돌의 정본은 헌법 제1~3조(경고 후 재확인 게이트) — [[00_Harness/ai-master-constitution.md]].

같은 세션 내에서 지시가 충돌할 때:
```
1. 가장 최근 지시가 우선
2. 명시적 지시 > 묵시적 기대
3. 구체적 지시 > 일반적 지시
4. 모호하면 가정을 명시하고 진행하거나 확인 질문
```

---

## 7. 금지 패턴 (프롬프트 응답에서)

```
❌ 같은 내용 재진술 (한 응답 내에서 반복)
❌ "이렇게 하시면 될까요?" 반복 확인 (가정 명시 후 진행)
❌ 완료 보고 후 불필요한 추가 설명
❌ 사용자가 읽을 수 있는 코드를 다시 설명하는 긴 주석
❌ 요청하지 않은 기능, 개선, 제안을 본문에 섞기 (→ 완료 보고 후 별도 언급)
```

---

## 8. 한국어 / 영어 사용 기준

```
한국어 기본:
- 사용자에게 하는 설명, 안내, 요약, 보고
- Work diary, 인수인계 문서
- 사람이 읽는 Vault 문서

영어 유지:
- 코드, 함수명, 변수명, 클래스명
- CLI 명령어, 파일 경로, API 이름
- 오류 메시지 원문 (→ 한국어 해설 병기)
- 사용자가 영어 산출물 명시한 경우
```

---

*최초 작성: YYYY-MM-DD*
