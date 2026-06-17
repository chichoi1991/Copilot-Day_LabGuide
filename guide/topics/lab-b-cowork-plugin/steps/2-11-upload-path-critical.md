업로드: **M365 Admin Center → Copilot → Agents → All agents → Upload agent**.
Teams admin center 로 올리지 말 것 — lenient 검증이라 `agentSkills`/`agentConnectors` 같은 unknown
root 속성을 **조용히 버려서** Teams 전용 앱으로 등록된다. M365 Admin Center 의 strict 검증만이
Cowork 에 제대로 바인딩한다.
성공 신호: 업로드 후 "Supported on" 에 Copilot 아이콘 표시, Categories 채워짐.
(이미 v1 이 같은 id 로 배포돼 있으면 "이미 배포됨" 거부가 날 수 있는데, v2 는 **새 GUID** 이므로
신규 등록된다.)
