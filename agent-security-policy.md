---
category: "AI-Governance"
major_category: "AI-Governance"
middle_category: "Harness"
minor_category: "Security"
category_path: "AI-Governance/Harness/Security"
tags:
  - "human/korean"
  - "human/warning"
  - "human/decision"
  - "harness"
  - "security"
  - "category/ai-governance"
  - "category/ai-governance/harness"
  - "category/ai-governance/harness/security"
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
  - "00_Harness/ai-master-constitution.md"
---
# <span style="color:#dc2626">보안 정책 (Security Policy)</span>
# Harness Engineering v1

Tags: #human/korean #human/warning #human/decision #harness #security
관련 문서: [[00_Harness/harness-engineering-index.md]] · [[00_Harness/harness-change-history.md]] · {{SHARED_HUB}} · {{BOOT_PROTOCOL}} · [[00_Harness/ai-master-constitution.md]]

AI 에이전트가 반드시 준수해야 하는 보안 규칙.
ai-master-constitution.md 제2조의 세부 구현이다.




## 1. 절대 금지 (저장 금지 항목)

어떤 형태로도 Markdown, 코드, 주석, 로그에 저장하지 않는다:

```
❌ API key / API secret
❌ OAuth token, refresh token, access token
❌ password, passphrase
❌ private key (RSA, EC, SSH)
❌ cookie, session token
❌ 주민번호, 여권번호, 운전면허번호
❌ 전화번호, 이메일 ({{ORG_NAME}} 직원·고객)
❌ 실명, 생년월일 (비식별 처리 전)
❌ 신용카드 번호, 계좌번호
❌ {{ORG_NAME}} 내부 IP, 내부 도메인, 서버 정보 (공개 승인 전)
```

---

## 2. 코드 보안 규칙

### 입력값 검증

```python
# ❌ 금지 패턴
query = f"SELECT * FROM users WHERE id = {user_input}"

# ✅ 안전 패턴
query = "SELECT * FROM users WHERE id = %s"
cursor.execute(query, (user_input,))
```

```python
# ❌ 금지 패턴 (path traversal)
file_path = os.path.join(base_dir, user_filename)

# ✅ 안전 패턴
file_path = os.path.realpath(os.path.join(base_dir, user_filename))
if not file_path.startswith(os.path.realpath(base_dir)):
    raise ValueError("경로 이탈 감지")
```

### 환경 변수 사용

```python
# ❌ 금지
API_KEY = "sk-abc123..."

# ✅ 안전
import os
API_KEY = os.environ.get("OPENAI_API_KEY")
if not API_KEY:
    raise RuntimeError("OPENAI_API_KEY 환경 변수가 설정되지 않았습니다")
```

### 의존성 보안

```
✅ 최신 안정 버전 사용
✅ requirements.txt에 버전 고정
✅ 알려진 취약점이 있는 버전 사용 금지
✅ 불필요한 패키지 포함 금지
```

---

## 3. 파일 시스템 보안

```
✅ 파일 작업 전 경로 유효성 확인
✅ 심볼릭 링크 악용 방지 (realpath 사용)
✅ 임시 파일은 작업 후 정리
❌ 세계 쓰기 가능 권한 (777) 설정 금지
❌ Vault 외부에 민감 데이터 파일 저장 금지
```

---

## 4. 외부 통신 보안

```
✅ HTTPS만 사용 (HTTP 금지)
✅ 인증서 검증 비활성화 금지 (verify=False)
✅ 외부 전송 전 데이터에 PII 포함 여부 확인
❌ 사용자 승인 없이 외부 API로 데이터 전송 금지
❌ 로그에 API 응답 원문 전체 저장 금지 (민감 데이터 포함 가능)
```

---

## 5. 조직 기밀 처리 ({{ORG_NAME}})

```
✅ {{ORG_NAME}} 기밀은 Vault 내부에서만 처리
✅ 기밀 문서는 {{CONFIDENTIAL_DIR}} 폴더에만 저장
❌ {{ORG_NAME}} 직원 정보, 사업 계획, 미공개 기술을 Vault 외부 업로드 금지
❌ AI 응답에 {{ORG_NAME}} 기밀 정보 포함 금지 (공개 저장소 등)
```

기밀 정의 기준: `{{CONFIDENTIAL_PROFILE_DOC}}` (기밀 범위·등급을 정의해 둔 문서)

**visibility 게이트 연결:** 기밀 보호의 집행 정본은 [[00_Harness/file-operations-rules.md]] §4의 visibility 게이트(`public`|`internal`|`pii`, 기본 `internal`)다. **`{{CONFIDENTIAL_DIR}}` 및 기밀 문서를 신규 생성할 때는 `visibility: "pii"`를 명시해야 한다** (기본값에 의존하지 말 것). 발행(예: Quartz 같은 정적 사이트, {{PUBLISH_SITE}})은 `visibility: public`인 문서만 허용한다.

---

## 5a. 봉인 저장소

발행 전면 금지 경로 목록 `{{SEALED_PATHS}}`에 등재된 저장소 및 그 파생물은 어떤 외부 발행·업로드·git remote 대상도 아니다. 요청이 오면 진행 대신 봉인과 충돌함을 보고한다.

---

## 6. 의료 데이터 보안 (의료 관련 프로젝트 시 적용)

> 의료 관련 프로젝트를 진행하는 경우 아래 규칙을 적용한다.

```
✅ 의료 판단은 지원(Support) 정보로만 제공
❌ 진단 확정 또는 처방 자동화로 표현 금지
❌ 환자 데이터에 PII 포함 금지 (비식별 처리 필수)
❌ 의료 데이터를 외부 AI API로 전송 금지 (로컬 추론만)
```

---

## 7. 보안 위반 감지 시 즉각 조치

```
1. 현재 작업 즉시 중단
2. 위반 내용과 범위 파악
3. 사용자에게 즉시 알림 (P0 등급)
4. 노출된 경우: revoke/rotation 방법 안내
5. Markdown에 포함된 경우: 해당 내용 제거
6. git history에 포함된 경우: git-filter-repo 안내
7. 재발 방지 방법 작성 후 work diary에 기록
```

---

## 8. .gitignore 권고 항목

코드 프로젝트에 항상 포함:

```
.env
.env.local
.env.*.local
*.key
*.pem
*.p12
*.pfx
secrets/
credentials/
__pycache__/
*.pyc
.DS_Store
```

---

*최초 작성: YYYY-MM-DD*
