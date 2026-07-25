---
created_at: "YYYY-MM-DD"
updated_at: "YYYY-MM-DD"
project: "harness"
type: "checklist"
tool: "manual"
status: "active"
visibility: "internal"
source_path: ""
related:
  - "00_Harness/harness-engineering-index.md"
  - "verification-methodology.md"
---

# 격리 검증 체크리스트 (V1~V9)

하네스가 스스로 정한 규칙을 지키고 있는지 **실측으로만** 판정하는 체크리스트입니다.
분기마다 한 번, 또는 하네스를 크게 고친 직후에 실행하십시오.

## 실행 규칙 (중요)

1. **깨끗한 세션이 수행합니다.** 하네스를 작성·수정한 세션은 이 검증을 수행할 수 없습니다 — 자기 채점은 결함을 못 잡습니다.
2. 검증 세션은 **파일을 수정하지 않습니다.** PASS/FAIL 판정표와 재작업 목록만 반환합니다.
3. 판정 근거는 grep·파일 크기·해시 같은 실측 출력이어야 합니다. "읽어보니 괜찮음"은 판정이 아닙니다.
4. FAIL은 재작업 지시서로 회수하고, 회수 후 해당 항목만 재검증합니다.

## 체크 9종

아래 명령은 예시(PowerShell 기준)입니다. 자기 환경의 경로로 치환해 쓰십시오.

```powershell
# V1. 우선순위 서열의 단일 정본 — 헌법 외에 독자적 서열 서술이 0건인가?
#     (자기 환경에서 서열을 표현하는 관용구를 패턴에 추가할 것)
Select-String -Path {{VAULT_ROOT}}\00_Harness\*.md -Pattern "1순위|우선순위는"

# V2. 낡은 기준 잔존 — 폐기한 enum·수치·규칙의 옛 값이 0건인가?
#     (예: 구 type 목록, 구 파일명 단어수 기준)

# V3. 파일 수·보관 정합 — 00_Harness 파일 수가 인덱스 표기와 같은가?
#     보관 문서는 전부 아카이브 폴더에 있고 status: archived + 정본 배너를 가졌는가?
(Get-ChildItem {{VAULT_ROOT}}\00_Harness -Filter *.md).Count

# V4. 죽은 링크 — 인덱스·진입 파일의 모든 [[위키링크]] 대상이 실존하는가?

# V5. 크기 예산 — 부팅 필수 2문서(헌법 + boot-manifest) 합계가 15KB 이하인가?
#     신설·대폭 수정 문서가 각자의 크기 예산 안에 있는가?

# V6. 핵심 안전장치 존재 — 다음 문구가 지정 문서에서 검출되는가?
#     "사용자 승인이 아니다"(무인 정책) / visibility 게이트 / checkpoint 규칙 /
#     봉인 경로 목록 / 검색 한계 고지

# V7. frontmatter 자기규칙 준수 — 정본이 요구하는 필수 필드(visibility 포함)를
#     00_Harness 전 파일이 실제로 가졌는가? updated_at이 실제 수정일과 일치하는가?
Select-String -Path {{VAULT_ROOT}}\00_Harness\*.md -Pattern "^visibility:"

# V8. 진입 문구 동일성 — 전역 진입 파일들(CLAUDE.md/GEMINI.md/AGENTS.md)의
#     하네스 연결 헤더가 문자 단위로 동일한가? (해시 비교)

# V9. 진입 순서 일관성 — 워크플로 다이어그램·boot-manifest·진입 파일 헤더·부팅
#     프로토콜이 전부 같은 진입 순서를 말하는가?
```

## 판정표 형식

| 항목 | 판정 | 실측 근거 |
|---|---|---|
| V1 | PASS / FAIL | (명령 출력 요지) |
| … | | |

**판정 기준:** PASS = 기대값과 실측 일치. FAIL = 문서 자신의 정본 규칙과 실제 상태가 어긋남.

> 참고: 이 체크리스트의 원형이 처음 실전 투입됐을 때, 작성자와 검토자 둘 다 놓친 결함 2건(정본의 자기규칙 위반, 부팅 연결 고리 누락)을 잡아냈습니다. V7·V9가 그 흔적입니다.
