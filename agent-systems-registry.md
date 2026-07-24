---
created_at: "YYYY-MM-DD"
updated_at: "YYYY-MM-DD"
project: "harness"
type: "spec"
tool: "claude"
status: "active"
visibility: "internal"
source_path: ""
related:
  - "00_Harness/ai-master-constitution.md"
  - "00_Harness/unattended-operations-policy.md"
  - "00_Harness/harness-engineering-index.md"
  - "{{BOOT_PROTOCOL}}"
---

# Agent Systems Registry

Tags: #harness #systems #registry

현행 가동 중인 시스템 명부. 각 시스템은 표 1행 + 필요 시 1~2줄만 서술한다 — 상세 규칙은 정본 링크로 위임하며 여기 복제하지 않는다. 등재되지 않은 시스템은 다른 하네스 문서에서 참조할 수 없는 것으로 간주하고, 새 시스템 가동 시 이 명부부터 갱신한다.

## 시스템 유형별 등재 형식

각 시스템은 아래 4열 표 형식으로 1행씩 등재한다. 아래 3행은 **플레이스홀더 예시**이며, 자기 환경의 실제 시스템으로 교체한다.

| 시스템 | 위치 | 역할 | 정본/근거 |
|---|---|---|---|
| 핸드오프 릴레이 (ORDER/REPORT 파일 교환) | `{{RELAY_ROOT}}` (order/report/archive/state 등 하위 폴더) | 기획 세션→워커 세션 ORDER/REPORT 비동기 위임 | [[00_Harness/agent-handoff-protocol.md]] |
| Vault RAG 의미 검색 | `{{RAG_DIR}}` (인터프리터: `{{RAG_PYTHON}}`) | 파일명 검색보다 우선하는 의미 검색 (설계 상수 예: 120,000바이트 초과 파일은 색인 스킵) | [[00_Harness/task-execution-protocol.md]] |
| 산출물 HTML 수집 폴더 | `{{HTML_DOCS_DIR}}` | 시각화·보고서 standalone HTML의 단일 수집처 + 색인 빌더 | [[00_Harness/file-operations-rules.md]] §해당 절 |

등재 대상이 되는 시스템 유형의 예 (해당 시스템을 운용 중일 때만 1행씩 추가):

- 서브에이전트 로스터(예: chief-director, researcher, builder-1 …) — 세션 내 분업 체계
- 핸드오프 릴레이(ORDER/REPORT 파일 교환)
- 외부 AI 교차검증 릴레이
- 의도 원장(자동 갱신되는 사용자 의도 프로필)
- 반성 로그(실패·교훈 누적 장부) — 세션 시작 시 자동 주입형이면 그 사실을 역할란에 명시
- 에이전트 훅(라벨·주입·가드 등 — 자동 실행 환경의 실질 안전장치)
- 사용자 스킬·커맨드 모음
- 공개 발행 사이트 `{{PUBLISH_SITE}}` (예: Quartz 같은 정적 사이트) — `visibility: public` 게이트 적용
- 발행 전면 금지 경로 `{{SEALED_PATHS}}` — 발행·업로드·링크·git remote 전면 금지
- 클라우드 열람 허브 `{{CLOUD_VIEWER}}`

## 사용 원칙

- 이 표는 시스템 존재를 등재하는 명부다. 운영 규칙(무인 실행 거버넌스 등)의 정본은 [[00_Harness/unattended-operations-policy.md]].
- 신규 시스템을 가동하면 이 표에 1행 추가한다(별도 ORDER 불필요, 정본 문서 링크 필수).
- 이 문서에 규칙 본문을 복제하지 않는다 — 정본 링크가 없으면 등재하지 않는다.
