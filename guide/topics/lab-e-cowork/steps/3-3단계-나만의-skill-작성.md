### SKILL.md 기본 구조

```markdown
---
name: skill-name-kebab
description: 이 스킬이 언제 실행되어야 하는지 한 줄 설명 (Copilot이 트리거 판단에 사용)
triggers:
  - "트리거 문장 예시 1"
  - "트리거 문장 예시 2"
---

# 스킬 제목

## 목적
이 스킬이 하는 일을 간단히 설명합니다.

## 실행 지침
Copilot이 이 스킬을 실행할 때 따라야 할 구체적인 지침을 작성합니다.

1. 첫 번째 단계
2. 두 번째 단계
3. 결과물 형식

## 출력 형식
원하는 결과물의 형식이나 템플릿을 정의합니다.
```

\---

### 방법 1: 채팅으로 SKILL 등록 

채팅창에서 아래 프롬프트로 직접 스킬을 만들고 저장할 수 있습니다.

#### Step A — 스킬 생성 요청

```
보고서 PPT 템플릿을 만드는 새 스킬을 만들어줘.

이름: exec-report-ppt
설명:  보고용 EV 자동차 품질팀 PPT 템플릿
트리거: "대표 보고 PPT 만들어줘", "임원 보고 슬라이드 만들어줘", "대표 보고서 PPT 생성해줘", "경영 보고 PPT 만들어줘", "보고 자료 PPT로 만들어줘"

지침:
이 스킬이 실행되면 아래 사양에 따라 한국 대기업 보고서 스타일의 PowerPoint 파일을 생성한다. 사용자에게 별도로 묻지 않고 M365 데이터를 먼저 검색한 뒤 슬라이드를 채운다. 데이터가 없는 항목은 삭제하지 말고 편집 가능한 플레이스홀더(\[내용 입력 필요])로 남긴다.



1. 기본 조건

   1. 슬라이드 크기: A4 Landscape, 11.69 × 8.27 inch.
   * 전체 배경: 흰색. 사진 배경 또는 장식 이미지는 사용하지 않는다.
   2. 모든 텍스트, 도형, 표, 차트는 PPT에서 직접 편집 가능한 객체로 만든다. 배경 이미지로 고정하지 않는다.
   3. 폰트: Malgun Gothic 혹은 맑은 고딕
   4. 색상: Deep Blue #1844A3, Accent Green #009653, Issue Background #EFF7FF, Pale Blue #F6FAFF, Body Gray #3F4147, Mid Gray #7B8190, Line Gray #DDE5EE.

|역할|이름|HEX|
|주조색|Deep Blue|#1844A3|
|강조색|Accent Green|#009653|
|슬라이드 배경|Background|#FFFFFF|
|연한 배경|Pale Blue|#F6FAFF|
|본문 텍스트|Body Gray|#3F4147|
|보조 텍스트|Mid Gray|#7B8190|
|구분선·테두리|Line Gray|#DDE5EE|

2. 공통 헤더/푸터 규칙 (전 슬라이드 동일 적용)

  상단 4단 탭 네비게이션

* 위치: 상단 여백 y=0.22in
* 탭 텍스트: "I. 사업 배경" / "II. 추진 전략" / "III. 수행 방법" / "IV. 제안 사항"
* 탭 시작: x=0.76in, 탭 폭: 1.56in, 탭 간격: 0.13in, 탭 높이: 0.22in

  우측 상단 문서 정보

* 위치: x=9.1in 근처, 우측 정렬
* 내용: 문서명 + 페이지 번호
* 서식: 5.7pt, 회색

  헤더 하단 구분선

* 위치: y=0.62in, x=0.72in \~ 10.97in
* 색상: Accent Green(#009653), 선 굵기 1.3pt

  하단 푸터

* 위치: y=7.85in
* 왼쪽: Confidential – For Proposal Only — 4.9pt, Mid Gray
* 오른쪽: Copyright 문구 — 4.9pt, Mid Gray
* 구분선: 얇은 그린-그레이 수평선

3. 제목 영역 규칙 (공통)

* 본문 시작: x=0.78in
* 섹션 라벨: y=0.83in, 7pt, Body Gray(#3F4147)
* 대제목: y=1.03in, 20–22pt, Bold, Deep Blue(#1844A3)
* 설명문: y=1.46in

  * 좌측: Accent Green 세로 바 (폭 0.05in × 높이 0.18in)
  * 설명문: 10pt, Body Gray(#3F4147)

4. Why? 박스 규칙

  * 위치·크기: x=0.78in, y=1.82in, w=10.13in, h=0.38in
  * 배경: #EFF7FF
  * 좌측 라벨: "Why?", Deep Blue Bold 8pt, x=0.91in
  * 메시지 2개:

    * 첫 번째: x=1.78in
    * 두 번째: x=6.32in
  * 각 메시지는 ·로 시작, 8.4pt Bold, 색상 #34405A

5. 핵심 컴포넌트 규격

  * 2×2 이슈 그리드: 중앙 세로선 x=5.94in, y=2.42\~6.93in. 좌측 모듈 x=0.84in w=4.78in, 우측 모듈 x=6.30in w=4.63in. 상단 y=2.54in, 하단 y=5.02in, 모듈 높이 1.55in.
  * 모듈: 번호 박스 0.37×0.29in, 번호 01/03은 Deep Blue, 02/04는 Green. 제목은 12.8pt Bold Deep Blue. 제목 아래 얇은 #DDE5EE 라인. 본문 bullet은 8.7pt Body Gray.
  * KPI Card: 흰 배경, #DDE5EE 라인, 값은 15pt Bold, 라벨은 7pt Mid Gray.
  * 표: 헤더 배경 #EFF7FF, 헤더 텍스트 Deep Blue Bold, 본문은 흰색/#F8FAFC 교차 배경.

6. 생성해야 할 슬라이드 구성

  * 총 10장으로 만든다. 제목, 목차, 결론 슬라이드를 반드시 포함한다.
  * 1\)표지, 2) 목차, 3) 섹션 구분, 4) 사업 추진 배경 2×2 이슈 그리드, 5) Executive Summary, 6) 현황 진단 대시보드 및 Gap 분석, 7) 추진 전략 방향, 8) 대안 비교 및 선정, 9) 기대효과, 10) 끝맺음.

7. 문체와 콘텐츠 규칙

  * 한국 대기업 보고서 톤으로 작성한다.
  * 슬라이드당 핵심 메시지는 1개, 보조 메시지는 2\~4개로 제한한다.
  * 텍스트는 실제 보고서 예시 문장으로 채우되, 사용자가 쉽게 바꿀 수 있도록 과도하게 길게 쓰지 않는다.
  * 제목은 Deep Blue Bold, 강조 수치나 실행 포인트는 Green을 사용한다.

8. 품질 검수

```

#### Step B — 스킬 저장 확인

Copilot이 SKILL.md 초안을 보여주면:

```
이 스킬을 등록해줘
```

> \*\*OneDrive 경로:\*\* `Documents/Cowork/<skill-name>/SKILL.md` 에 자동 저장됩니다.

스킬 품질 점수 채점 확인하고 점수를 더 올리고싶으면

```
이 스킬 최적화해줘
```

\---

### 방법 2: OneDrive에 직접 SKILL.md 파일 업로드

Copilot 채팅 없이 파일을 직접 작성해 업로드하는 방법입니다.

#### Step A — .md 파일 OneDrive에 업로드

**방법 A (웹 브라우저):**

1. [onedrive.cloud.com](https://onedrive.cloud.microsoft) 접속
2. `My files > Documents > Coworker > skills` 폴더로 이동
3. `새로 만들기 > 폴더` 로 스킬 이름 폴더 생성 (예: `exec-report-ppt`)
4. 생성한 폴더 안에 `SKILL.md` 파일 업로드

**방법 B (파일 탐색기):**

1. 동기화된 OneDrive 폴더 열기
2. `Documents > Coworker > skills` 경로로 이동
3. 스킬 폴더 생성 후 `SKILL.md` 복사

> \*\*주의:\*\* 폴더명은 `name:` 필드의 값과 일치시키는 것을 권장합니다.


\---
