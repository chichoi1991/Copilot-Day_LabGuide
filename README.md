# Copilot & Agent Day Seoul — 실습 랩 가이드

> **2026년 6월 18일**에 진행된 **Copilot & Agent Day** 실습 랩 가이드 모음입니다.
> 각 랩의 배포 교안, 원본 교안, 샘플/솔루션 파일을 한 곳에서 내려받을 수 있습니다.

머신리더블 메타데이터는 [`labs.json`](labs.json)에 정리되어 있습니다. (스키마: [`labs.schema.json`](labs.schema.json)) — 향후 배포 웹페이지가 이 파일을 그대로 사용합니다.

## 랩 목록

| # | 제목 | 담당자 | 가이드 문서 | 원본 다운로드 | 실습 링크 |
|---|---|---|---|---|---|
| A | [가상 임원회의 에이전트 (AI Board Room)](labs/lab-a-boardroom) | 최원영 ([@chichoi1991](https://github.com/chichoi1991)) | [배포 교안](guide/topics/lab-a-boardroom) | [원본 교안](labs/lab-a-boardroom/original/AI%20의사결정%20지원%20에이전트_LAB%20메뉴얼_202604.docx) | [실습 바로가기](guide/topics/lab-a-boardroom) |
| B | [Copilot Cowork 플러그인](labs/lab-b-cowork-plugin) | 조윤호 | [배포 교안](guide/topics/lab-b-cowork-plugin) | [원본 교안](labs/lab-b-cowork-plugin/original/learner-prompt-guide.md) | [실습 바로가기](guide/topics/lab-b-cowork-plugin) |
| C | [Copilot Studio New Workflow로 뉴스 레포트 자동화](labs/lab-c-news-workflow) | 이영서 | [배포 교안](guide/topics/lab-c-news-workflow) | [원본 교안](labs/lab-c-news-workflow/original/DailyBrief_HandsOn_Guide.docx) | [실습 바로가기](guide/topics/lab-c-news-workflow) |
| D | [MCP로 확장하는 에이전트 아키텍쳐](labs/lab-d-mcp) | 김유연 | [배포 교안](guide/topics/lab-d-mcp) | [원본 교안](labs/lab-d-mcp/original/cps-lab-guide-kr.md) | [실습 바로가기](guide/topics/lab-d-mcp) |
| E | [Copilot Cowork 활용](labs/lab-e-cowork) | 장보윤 | [배포 교안](guide/topics/lab-e-cowork) | [원본 교안](labs/lab-e-cowork/original/Copilot%20Cowork%20-%20HandsOn%20Lab%20guide.md) | [실습 바로가기](guide/topics/lab-e-cowork) |
| F | [우리 조직 양식을 이용한 보고서 작성](labs/lab-f-report) | 김진우 | [배포 교안](guide/topics/lab-f-report) | _템플릿 기반 (원본 교안 없음)_ | [실습 바로가기](guide/topics/lab-f-report) |

> 담당자 GitHub 주소가 확인되지 않은 항목은 추후 [`labs.json`](labs.json)의 `owner.github` 필드에 추가하면 표와 웹페이지에 함께 반영됩니다.

## 저장소 구조

```
Copilot-Day_LabGuide/
├── README.md              ← 이 문서 (대문 / 랩 카탈로그)
├── labs.json              ← 랩 메타데이터 (제목·담당자·다운로드 링크) — 웹페이지용 단일 진실의 원천
├── labs.schema.json       ← labs.json 스키마
├── labs/                  ← 랩별 배포 자료
│   └── lab-x-.../
│       ├── README.md      ← 랩별 안내 + 다운로드 링크
│       ├── original/      ← 원본 교안
│       ├── samples/       ← 샘플 파일
│       └── solution/      ← 솔루션 파일 (Power Platform 솔루션 등)
├── guide/                 ← 이미 배포된 교안 (단계별 가이드 + 이미지)
│   ├── guides.json        ← 토픽 인덱스
│   └── topics/lab-x-.../  ← 랩별 단계 문서(steps/*.md)
└── dist/
    └── guide.bundle.json  ← 포털용 가이드 번들
```

## 각 랩 자료 구성

각 `labs/<lab-id>/` 폴더는 다음을 포함합니다.

- **배포 교안** — 이 저장소 `guide/topics/<lab-id>/`의 단계별 가이드
- **원본 교안** — 발표자가 제공한 원본 문서(docx/md)
- **샘플 파일** — 실습용 데이터·템플릿
- **솔루션 파일** — Power Platform 솔루션(zip) 등 가져오기용 파일

자세한 내용은 각 랩 폴더의 `README.md`를 참고하세요.
