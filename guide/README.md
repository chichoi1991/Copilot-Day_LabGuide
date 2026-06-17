# Guide Authoring Reference

> 이 문서는 다음 세션의 AI/사람 작성자가 가이드 콘텐츠를 추가·수정할 때 참고하는 **단일 진실의 원천**입니다. 변경 시 반드시 이 문서도 함께 업데이트하세요.

---

## 1. 디렉토리 구조

```
guide/
├── README.md                              ← 이 문서
├── guides.json                            ← 마스터 인덱스: 토픽 목록 + defaultTopic
├── env.json                               ← 공통 환경 정보 (region/vmSize/os)
└── topics/
    ├── 01-azure-basics/
    │   ├── manifest.json                  ← 이 토픽의 메타 + steps OR chapters
    │   └── steps/*.md                     ← 본문
    ├── 02-copilot-studio/                 ← 챕터 구조 예시
    │   ├── manifest.json
    │   └── steps/ch1-01-*.md, ch2-01-*.md ...
    └── 03~06-.../
```

빌드 결과는 `dist/guide.bundle.json` 단일 파일이며, 포털(`/var/www/html/index.html`)이 `fetch('/guide.bundle.json')`로 로드합니다.

---

## 2. 작업 흐름 (5단계)

가이드 콘텐츠를 추가/수정할 때 항상 이 순서로:

1. **MD 파일 작성/수정** — `guide/topics/<토픽폴더>/steps/*.md`
2. **manifest.json 동기화** — 파일이 새로 생기거나 제목/체크가 바뀌면 반드시 manifest 업데이트. **manifest가 진실의 원천 — 파일이 있어도 manifest에 등록 안 하면 화면에 안 보임**
3. **로컬 빌드 검증** — `node scripts/build-guide-bundle.js` (오류 메시지 확인)
4. **커밋 & 푸시** — GitHub Action이 `dist/guide.bundle.json`을 자동 재빌드·커밋
5. **(선택) 즉시 배포** — 워크샵 직전 반영하려면 로컬에서:
   ```powershell
   powershell -ExecutionPolicy Bypass -File scripts\deploy-guide.ps1
   ```
   (포털 HTML/CSS 변경이 아니면 `_deploy3.sh` 재실행은 불필요)

---

## 3. `guides.json` (마스터 인덱스)

```json
{
  "version": "2.0",
  "defaultTopic": "01-azure-basics",
  "topics": [
    { "id": "01-azure-basics",   "title": "Azure 기초",            "icon": "☁️", "order": 1 },
    { "id": "02-copilot-studio", "title": "Copilot Studio Agent", "icon": "🤖", "order": 2 }
  ]
}
```

| 필드 | 필수 | 설명 |
|---|---|---|
| `version` | ✓ | 스키마 버전 (현재 `"2.0"`) |
| `defaultTopic` | ✓ | 페이지 로드 시 자동 선택되는 토픽 `id` |
| `topics[].id` | ✓ | 폴더명과 **정확히** 일치 (`topics/<id>/`) |
| `topics[].title` | ✓ | 드롭다운에 표시될 이름 |
| `topics[].icon` | – | 단일 이모지 (드롭다운 좌측 표시) |
| `topics[].order` | ✓ | 정렬 순서 (오름차순) |

### 새 토픽 추가 절차
1. `guide/topics/07-신규/steps/` 폴더 생성
2. `guide/topics/07-신규/manifest.json` 생성
3. `guides.json`의 `topics[]` 배열에 항목 추가
4. (선택) `defaultTopic` 변경

---

## 4. `env.json` (공통 환경 정보)

우측 패널 **"환경 정보" 탭** 하단에 표시. 모든 토픽 공통.

```json
{
  "region": "Korea Central",
  "vmSize": "Standard_D4s_v3",
  "os": "Windows 11 Pro 24H2"
}
```

워크샵용 인프라가 바뀌면 이 파일만 수정.

---

## 5. 토픽별 `manifest.json`

**두 가지 형태 중 하나만** 사용합니다 (둘 다 정의하면 chapters 우선).

### 5-A. 평면 구조 (짧은 가이드 — step 4~6개 이내 권장)

```json
{
  "title": "Azure 기초",
  "description": "Azure CLI로 리소스 그룹을 생성·관리합니다",
  "estimatedMinutes": 30,
  "steps": [
    { "id": "1", "title": "환경 확인",         "file": "steps/01-env.md",     "checks": ["VM 바탕화면 확인", "마우스/키보드 동작 확인"] },
    { "id": "2", "title": "Azure CLI 확인",    "file": "steps/02-cli.md",     "checks": ["az --version 확인", "az login 완료"] }
  ]
}
```

### 5-B. 챕터 구조 (긴 가이드 — step이 많거나 단계가 논리적으로 묶일 때)

```json
{
  "title": "Copilot Studio Agent",
  "description": "Copilot Studio로 첫 Agent를 만들고 토픽·액션을 추가합니다",
  "estimatedMinutes": 90,
  "chapters": [
    {
      "id": "ch1",
      "title": "Chapter 1: 환경 준비",
      "steps": [
        { "id": "1.1", "title": "Copilot Studio 접속",  "file": "steps/ch1-01-access.md",  "checks": ["로그인 성공"] },
        { "id": "1.2", "title": "테넌트/라이선스 확인", "file": "steps/ch1-02-license.md", "checks": ["라이선스 확인"] }
      ]
    },
    {
      "id": "ch2",
      "title": "Chapter 2: Agent 만들기",
      "steps": [
        { "id": "2.1", "title": "새 Agent 생성", "file": "steps/ch2-01-create.md", "checks": ["Agent 생성"] }
      ]
    }
  ]
}
```

### 필드 레퍼런스

| 필드 | 필수 | 타입 | 설명 |
|---|---|---|---|
| `title` | ✓ | string | 토픽 제목 (drop-down title 보다 우선순위 낮음 — guides.json의 title이 보임) |
| `description` | – | string | 토픽 셀렉터 아래에 표시되는 한 줄 설명 |
| `estimatedMinutes` | – | int | "⏱ 예상 N분"으로 표시 |
| `steps` **또는** `chapters` | ✓ | array | 둘 중 하나만 선택 |
| `chapters[].id` | ✓ | string | 챕터 식별자 (UI 안 보임, 내부용) |
| `chapters[].title` | ✓ | string | 챕터 헤더로 표시 |
| `chapters[].steps` | ✓ | array | 챕터 내 step 배열 |
| `steps[].id` | ✓ | string | 스텝 번호 (예: `"1"`, `"1.1"`). step-num 원형 안에 그대로 표시됨 |
| `steps[].title` | ✓ | string | 스텝 헤더 |
| `steps[].file` | ✓ | string | `steps/...md` 상대 경로 (`manifest.json` 기준) |
| `steps[].checks` | – | string[] | 본문 하단 체크박스 라벨들 (빈 배열 가능) |

### step id 권장 컨벤션
- **평면 구조**: `"1"`, `"2"`, `"3"`, ... (단순 정수)
- **챕터 구조**: `"<챕터번호>.<스텝번호>"` 예: `"1.1"`, `"1.2"`, `"2.1"` (UI에서 step-num 원형에 그대로 표시되므로 한눈에 챕터 소속이 보임)

### 파일명 권장 컨벤션
- 평면: `01-env.md`, `02-cli.md`, `03-rg.md` (정렬 위해 두 자리 prefix)
- 챕터: `ch1-01-access.md`, `ch1-02-license.md`, `ch2-01-create.md` (챕터 prefix로 그룹 식별)

---

## 6. Markdown 작성 가이드

### 6-1. 기본 — 모든 일반 마크다운 지원
heading, list, bold, italic, link, table, blockquote, image, inline code 모두 정상 렌더링됩니다 (marked.js v15 사용).

### 6-2. 특수 코드블록 — VM 전송 버튼 자동 부착

```` markdown
```vm
az --version
```
````

위처럼 언어 태그를 **`vm`** 으로 지정하면 코드블록 우측에 **📋 VM 전송** 버튼이 자동 생성됩니다. 클릭 시:
1. 코드가 사용자 OS 클립보드에 복사됨
2. Guacamole iframe에 포커스 + Ctrl+V 키 이벤트 디스패치
3. 토스트 알림 표시

| 언어 태그 | VM 전송 버튼 | 용도 |
|---|---|---|
| `vm` | ✓ | **권장** — VM 안에서 실행할 명령 (PowerShell, bash, az 등 모두 OK) |
| `ps` / `powershell` | ✓ | (호환) PowerShell 전용 |
| `bash` / `sh` | ✓ | (호환) bash 전용 |
| 그 외 (`json`, `yaml`, ...) | ✗ | 일반 코드 표시만 |
| 언어 태그 없음 | ✗ | 일반 코드 표시만 |

**원칙**: VM 안에서 실행할 모든 명령은 ` ```vm `으로 감싸세요. 참가자가 직접 타이핑할 필요 없이 버튼 한 번으로 전송됩니다.

### 6-3. 강조 박스 (Info Box)

```markdown
> 응답에 `"provisioningState": "Succeeded"`가 보이면 성공입니다.
```

`>` 인용문은 청록색 좌측 보더가 있는 강조 박스로 렌더링됩니다. **중요한 안내·확인 메시지**에 사용하세요.

### 6-4. 외부 링크

```markdown
[Azure Portal 바로가기](https://portal.azure.com)
```
일반 마크다운 링크 — 새 탭이 아니라 같은 탭에서 열리므로 가급적 외부 사이트는 **본문에서 언급만 하고 VM 안 브라우저에서 열도록 안내**하는 게 안전합니다 (사용자 세션 유지).

### 6-5. 이미지

```markdown
![설명](https://...)
```
외부 호스팅 이미지는 그대로 작동. 리포 내 이미지를 쓰려면 별도 자산 디렉토리(예: `guide/assets/`)를 만들고 `_deploy3.sh`에서도 `/var/www/html/assets/`로 복사하도록 추가 작업 필요 (현재 미지원).

### 6-6. MD 파일 작성 체크리스트

- [ ] 한 step = 한 화면 정도로 짧게 (스크롤 1~2회 이내)
- [ ] VM 명령은 모두 ` ```vm ` 사용
- [ ] 중요한 확인 사항은 `>` 인용문으로 강조
- [ ] 외부 링크는 새 탭 가정하지 말기
- [ ] step에 대응하는 `checks` 배열을 manifest에 등록 (체크박스 = 진행률에 반영)
- [ ] 너무 길면 step을 쪼개거나 챕터 구조로 전환

---

## 7. 체크박스(`checks`) 동작

- 본문 아래에 자동으로 체크리스트가 렌더링됨
- 모든 체크가 완료되면 좌측 step-num 원형이 **녹색**으로 바뀜
- 상단 진행률 바가 `완료수 / 전체수`로 업데이트
- 토픽 전환 시 체크 상태는 **메모리에 유지**됨 (페이지 새로고침 시는 초기화 — localStorage 사용 안 함)

---

## 8. 빌드/배포 스크립트 위치

| 파일 | 용도 |
|---|---|
| `scripts/build-guide-bundle.js` | `guide/**` → `dist/guide.bundle.json` 빌드 (Node, 의존성 0) |
| `scripts/deploy-guide.ps1` | 빌드 + `vm-guacamole`에 `guide.bundle.json` 업로드 |
| `scripts/build-deploy3.ps1` | `portal/index.html` + nginx config → `_deploy3.sh` 생성 (포털 UI 변경 시) |
| `scripts/deploy-credentials.ps1` | M365 크리덴셜 + 사이드카 API 배포 (계정 변경 시) |
| `.github/workflows/build-guide.yml` | `guide/**` 푸시 시 자동 빌드 + `dist/` 커밋 |

---

## 9. 자주 발생하는 실수

| 증상 | 원인 | 해결 |
|---|---|---|
| 화면에 step이 안 보임 | manifest에 등록 안 함 | `manifest.json`의 `steps` / `chapters[].steps`에 항목 추가 |
| 빌드 에러: `md not found` | `file` 경로 오타 | `manifest.json`의 `file` 값과 실제 파일명 대조 |
| 빌드 에러: `topic manifest missing` | `guides.json`의 `id`와 폴더명 불일치 | 둘 중 하나를 수정해 일치시킴 |
| VM 전송 버튼이 안 나옴 | 코드블록 언어 태그가 `vm`/`ps`/`bash` 중 하나가 아님 | ` ```vm `으로 변경 |
| 화면이 안 바뀜 (배포했는데) | 브라우저 캐시 | `Ctrl + Shift + R`로 강제 새로고침 |
| 챕터가 안 보이고 step만 평면으로 나옴 | manifest에 `steps`와 `chapters` 둘 다 있음 | 둘 중 하나만 남김 (chapters 우선) |

---

## 10. 다음 세션을 위한 빠른 명령 모음

```powershell
# 1) 로컬에서 빌드만 검증
cd "Copilot-Day-Seoul-Agent-Workshop-Lab"
node scripts/build-guide-bundle.js

# 2) 빌드 + vm-guacamole에 즉시 배포
powershell -ExecutionPolicy Bypass -File scripts\deploy-guide.ps1

# 3) 포털 UI(index.html/nginx) 자체를 변경했을 때
powershell -ExecutionPolicy Bypass -File scripts\build-deploy3.ps1
$AZ='C:\Program Files\Microsoft SDKs\Azure\CLI2\wbin\az.cmd'
& $AZ vm run-command invoke -g RG-LAB-POC -n vm-guacamole `
  --command-id RunShellScript --scripts "@portal\_deploy3.sh" `
  --query "value[0].message" -o tsv

# 4) 배포 확인
curl https://guac-rpedskcplxlxw.koreacentral.cloudapp.azure.com/guide.bundle.json
```

---

## 11. 변경 시 이 문서 함께 업데이트해야 하는 경우

다음 변경 사항이 생기면 이 README도 반드시 함께 업데이트하세요 (그래야 다음 세션이 정확히 작업할 수 있음):

- 빌드 스키마 변경 (manifest 새 필드 추가, 등)
- 새 마크다운 확장 (` ```vm ` 외 새 언어 태그 등)
- 배포 스크립트 이름/경로 변경
- 포털 URL 변경
- 토픽 추가/삭제/이름 변경 → § 3 표 갱신
- 환경 (`env.json`) 구조 변경