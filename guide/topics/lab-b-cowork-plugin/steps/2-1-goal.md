> 아래 규칙 사이의 모든 내용을 그대로 복사해 에이전트에 붙여넣으세요.
> (코드·경로·도구 이름·MCP URL 등 영문 식별자는 절대 번역/변경하지 마세요.)

---

Microsoft 365 Copilot Cowork(Frontier) 플러그인 패키지 **"IT Support Insights v2"** 를 빌드한다.
이 패키지는 **2개의 remote MCP 커넥터**(IT 헬프데스크 티켓 + 가상 프로젝트 관리)와 **5개의 스킬**
(1개 분석 허브 + 4개 출력 스킬)을 묶어, 자연어 한 문장으로 티켓을 분석하고 그 결과를
HTML 대시보드 / PowerPoint / Word / 주간 보고 메일로 생성한다. 두 MCP를 **교차**해
"주차 사건 ↔ 연계 프로젝트"를 함께 분석하는 것이 핵심 차별점이다.
M365 Admin Center에 업로드 가능한 단일 ZIP 파일을 출력한다.
패키지는 M365 통합 앱 매니페스트 devPreview 스키마를 통과해야 하며, Teams 전용이 아니라
**Copilot Cowork에 바인딩**되어야 한다.
