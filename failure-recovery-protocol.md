---
category: "AI-Governance"
major_category: "AI-Governance"
middle_category: "Harness"
minor_category: "Protocol"
category_path: "AI-Governance/Harness/Protocol"
tags:
  - "human/korean"
  - "human/warning"
  - "harness"
  - "recovery"
  - "category/ai-governance"
  - "category/ai-governance/harness"
  - "category/ai-governance/harness/protocol"
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
  - "00_Harness/agent-security-policy.md"
  - "00_Harness/unattended-operations-policy.md"
---
# 실패 복구 프로토콜 (Failure Recovery Protocol)
# Harness Engineering v1

Tags: #human/korean #human/warning #harness #recovery
관련 문서: [[00_Harness/harness-engineering-index.md]] · [[00_Harness/harness-change-history.md]] · {{SHARED_HUB}} · {{BOOT_PROTOCOL}} · [[00_Harness/agent-security-policy.md]]

오류, 실패, 예외 상황이 발생했을 때 AI가 따르는 표준 복구 절차.




## 1. 오류 심각도 분류

| 등급 | 설명 | 예시 | 즉각 조치 |
|------|------|------|-----------|
| P0 — 긴급 | 데이터 손실, 보안 위반 | 파일 삭제됨, 시크릿 노출 | 즉시 작업 중단 + 사용자 알림 |
| P1 — 높음 | 작업 완전 차단 | 도구 오류, 환경 불가 | 대안 시도 → 없으면 사용자 알림 |
| P2 — 중간 | 일부 기능 불가, 품질 저하 | 일부 파일 생성 실패 | 성공한 항목 보고, 실패 항목 명시 |
| P3 — 낮음 | 경미한 불편 | 파일명 규칙 미준수 | 수정 후 계속 진행 |

---

## 2. P0 긴급 복구 절차

**데이터 손실 발생 시:**
```
1. 즉시 모든 작업 중단
2. 삭제된 파일명과 경로를 기록
3. 사용자에게 즉시 알림: "파일 삭제 발생: [경로], 복구 가능 여부 확인 필요"
4. git이 있으면 git status, git log로 복구 가능 여부 확인
5. 복구 방법 제시 (git checkout, 휴지통, 백업 등)
   → 대량 작업 전 checkpoint 커밋이 존재하면 git reset --hard <checkpoint> 일괄 복원이 1순위
   (예방 조치: 전역 진입 파일(CLAUDE.md/GEMINI.md/AGENTS.md)의 "대량 파일 작업 전 복구 지점 규칙" — 5개 초과 일괄 작업 전 checkpoint 필수)
6. 사용자 지시에 따라 복구 진행
```

**보안 위반 감지 시 (시크릿 노출 등):**
```
1. 해당 파일/코드 즉시 격리 (추가 배포 차단)
2. 사용자에게 즉시 알림: "보안 경고: [내용] 감지됨"
3. 노출된 시크릿의 revoke/rotation 방법 안내
4. 관련 파일에서 시크릿 제거
5. git history에 포함됐으면 git-filter-repo 사용 안내
```

---

## 3. P1 도구/환경 오류 복구

**도구 실행 실패 시:**
```
1. 오류 메시지 전체를 기록
2. 대안 도구로 재시도:
   - Read 실패 → PowerShell Get-Content
   - Write 실패 → Bash tee 또는 경로 확인
   - MCP 실패 → 직접 파일 접근
3. 대안도 실패하면 사용자에게 정확한 오류와 함께 보고
4. 작업 일부를 수동으로 진행할 수 있는지 안내
```

**환경 불일치 (경로 없음, 권한 없음):**
```
1. 실제 존재하는 경로 확인 (Glob, PowerShell)
2. 권한 문제면 사용자에게 수동 실행 방법 안내
3. 폴더가 없으면 생성 후 재시도 (단, 생성이 안전한 경우)
```

---

## 4. P2 부분 실패 처리

작업의 일부만 성공했을 때:
```
1. 성공한 항목과 실패한 항목을 명확히 분리
2. 각 실패 항목에 이유 명시
3. 부분 완료 상태를 work diary에 기록
4. 재시도 가능한 항목과 수동 처리 필요 항목 구분
5. 완료 보고에 "부분 완료" 명시
```

---

## 5. 오류 보고 형식

**핸드오프 릴레이(ORDER/REPORT 파일 교환) 워커 분기:** 릴레이 워커 세션은 사용자에게 직접 보고하는 대신, REPORT에 실패 등급(아래 형식)을 기록해 기획 세션의 검토(review) 게이트로 반환한다. 정본: [[00_Harness/unattended-operations-policy.md]].

```markdown
## 오류 보고

### 오류 등급: P0 / P1 / P2 / P3
### 발생 시각: YYYY-MM-DD HH:mm
### 오류 내용:
[오류 메시지 원문]

### 영향 범위:
[어떤 파일/기능에 영향]

### 시도한 복구:
1. ...
2. ...

### 현재 상태:
[복구됨 / 부분 복구 / 미해결]

### 사용자 조치 필요:
[필요한 경우 구체적 안내]
```

---

## 6. 재시도 규칙

```
✅ 재시도 가능: 네트워크 오류 (1회), 파일 잠금 (잠시 후 1회)
❌ 재시도 금지: 권한 오류 (→ 사용자 안내), 보안 오류 (→ 즉시 보고)
❌ 루프 재시도: sleep 후 무한 재시도 금지
```

---

## 7. 실패 기록 (Work Diary)

P0, P1 등급 오류는 work diary에 반드시 기록:
```
- 오류 등급과 내용
- 복구 방법과 결과
- 재발 방지를 위한 교훈
- 다음 세션에서 확인해야 할 사항
```
