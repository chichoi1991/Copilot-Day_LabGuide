# Hands-on Lab: Working with MCP in Copilot Studio Agents

> MCP로 확장하는 Agent 아키텍쳐: 답변을 넘어 실행까지

**총 세션 시간:** 30분
**목표:** Microsoft Learn MCP Server를 사용하는 간단한 Copilot Studio agent를 만든 다음, custom MCP servers를 사용하는 기존 agent를 사용해 봅니다.

---

## What you will build

이 랩에서는 다음을 진행합니다.

1. 새로운 Copilot Studio agent를 만듭니다.
2. Microsoft Learn MCP Server를 tool로 추가합니다.
3. 간단한 prompts로 agent를 테스트합니다.
4. custom MCP servers를 사용하는 prebuilt agent를 엽니다.
5. example prompts를 실행해 보고, MCP tools가 agent에 의해 어떻게 사용되는지 관찰합니다.

---

## Before you start

시작하기 전에 다음 항목을 준비했는지 확인해 주세요.

* 준비된 Copilot Studio lab environment에 대한 접근 권한
* 지원되는 브라우저
* 워크숍 진행자가 제공한 lab environment account

> 참고: Microsoft Learn MCP Server는 Copilot Studio에서 이미 기본 선택 항목으로 제공됩니다. 또한 Museum Curator Agent도 여러분의 environment에 이미 준비되어 있으므로, 별도로 배포할 필요가 없습니다.

---

## Simple explanation: What is MCP?

**MCP**는 **Model Context Protocol**의 약자입니다.

MCP는 AI agent가 외부 tools와 knowledge sources에 연결할 수 있게 해 주는 표준 방식이라고 생각하면 됩니다.

이 랩에서는 다음과 같이 이해하면 됩니다.

* **agent**는 Copilot Studio에서 생성됩니다.
* **MCP server**는 agent가 사용할 수 있는 tools 또는 정보를 제공합니다.
* agent는 사용자의 질문을 바탕으로 언제 해당 tools를 호출할지 결정합니다.

Microsoft Learn Docs MCP Server를 사용하면 agent가 Microsoft Learn 문서를 검색하고, Learn articles를 가져오고, Microsoft Learn의 code samples를 검색할 수 있습니다.

중요한 점은 다음과 같습니다.

* Copilot Studio는 **Streamable HTTP**를 통해 MCP를 지원합니다.
* SSE transport는 deprecated 되었으며, 2025년 8월 이후 Copilot Studio에서 지원되지 않습니다.

---

# Part 1: Create a new Copilot Studio agent with Microsoft Learn MCP

이 Part에서는 새로운 agent를 만들고 Microsoft Learn MCP Server에 연결합니다.

---

## Step 1: Open Copilot Studio

1. 브라우저에서 **Copilot Studio**를 엽니다.
2. lab account를 사용하여 로그인합니다.
3. 올바른 lab environment에 있는지 확인합니다.

environment selector가 보이면, 워크숍 진행자가 제공한 environment를 사용하고 있는지 확인합니다.

---

## Step 2: Create a new agent

1. **Create**를 선택합니다.
2. **New agent**를 선택합니다.

---

## Step 3: Set the agent name

아래 example values를 사용합니다.

### Agent name

```text
Microsoft Learn Guide
```

---

## Step 4: Add clear instructions to the agent

agent가 생성되면 agent의 **Instructions** 섹션을 엽니다.

### Copy and paste: Agent instructions

```text
당신은 유용한 Microsoft Learn guide입니다.

당신의 역할은 business users와 technical users가 신뢰할 수 있는 Microsoft documentation을 찾고, 이를 쉬운 언어로 이해할 수 있도록 돕는 것입니다.

사용자가 Microsoft products, services, features, configuration, licensing concepts, architecture 또는 implementation guidance에 대해 질문할 때는 다음을 따르세요.

1. 최신 Microsoft documentation이 필요한 경우 Microsoft Learn MCP tools를 사용하세요.
2. 답변을 명확하게 요약하세요.
3. 불필요한 technical jargon은 피하세요.
4. 답변이 product documentation에 의존하는 경우, 해당 정보가 Microsoft Learn에서 온 것임을 언급하세요.
5. 확실하지 않은 경우 추측하지 말고 Microsoft Learn을 확인해야 한다고 말하세요.

답변은 실용적이고 간결하게 유지하세요.
```

```text
You are a helpful Microsoft Learn guide.

Your role is to help business and technical users find reliable Microsoft documentation and understand it in simple language.

When the user asks about Microsoft products, services, features, configuration, licensing concepts, architecture, or implementation guidance:

1. Use the Microsoft Learn MCP tools when current Microsoft documentation is needed.
2. Summarise the answer clearly.
3. Avoid unnecessary technical jargon.
4. If the answer depends on product documentation, mention that the information comes from Microsoft Learn.
5. If you are not sure, say that you need to check Microsoft Learn instead of guessing.

Keep answers practical and concise.
```

**Save**를 통해 저장합니다.

---

## Step 5: Turn on generative orchestration

MCP를 사용하려면 Copilot Studio에서 generative orchestration이 필요합니다.

1. agent settings를 엽니다.
2. **Generative orchestration**을 찾습니다.
3. 이를 **On**으로 설정합니다.
4. 변경 사항을 저장합니다.

이미 켜져 있다면 다음 단계로 진행합니다.

---

## Step 6: Add the Microsoft Learn Docs MCP Server as a tool

1. agent의 **Tools** 페이지로 이동합니다.
2. **Add a tool**을 선택합니다.
3. **New tool**을 선택합니다.
4. **Model Context Protocol**을 선택합니다.
5. 사용 가능한 default options에서 Microsoft Learn Docs MCP Server를 선택합니다.

세부 정보를 수동으로 입력하라는 메시지가 표시되면 다음을 사용합니다.

### Server name

```text
Microsoft Learn MCP Server
```

### Server description

```text
Microsoft Learn documentation을 검색하고, Microsoft Learn articles를 가져오며, Microsoft code samples를 찾습니다.
```

### Server URL

```text
https://learn.microsoft.com/api/mcp
```

### Authentication

```text
None
```

6. **Create**를 선택합니다.
7. 메시지가 표시되면 **Create a new connection**을 선택합니다.

---

## Step 7: Confirm the tool was added

**Tools** 페이지에서 Microsoft Learn MCP Server가 agent의 tools list에 표시되는지 확인합니다.

MCP server에서 제공되는 tools 또는 resources가 보일 수 있습니다. 이들은 agent가 자동으로 사용할 수 있도록 제공됩니다.

---

## Step 8: Test the agent

preview panel을 열고 아래 prompts를 사용해 봅니다.

### Prompt 1

```text
Microsoft Copilot Studio가 무엇인가요? business decision maker를 대상으로 설명해 주세요.
```

```text
What is Microsoft Copilot Studio? Please explain it for a business decision maker.
```

### Prompt 2

```text
Copilot Studio에서 MCP를 사용할 때 어떤 transport type을 사용해야 하나요?
```

```text
What transport type should I use for MCP with Copilot Studio?
```

### Prompt 3

```text
cloud migration의 best practices를 요약해 주세요. 특히 financial services 관점에서 설명해 주세요.
```

```text
Summarize the best practices in cloud migration, especially for financial services.
```

### Prompt 4

```text
Copilot Studio agents에서 MCP를 사용하는 실용적인 use cases 세 가지를 알려 주세요.
```

```text
Give me three practical use cases for using MCP with Copilot Studio agents.
```

### Prompt 5

```text
Azure에서 SQL databases를 관리하는 options를 제시해 주세요.
```

```text
Provide options of managing SQL databases on Azure.
```

### The answer is too technical

다음을 추가해 봅니다.

```text
non-developer business audience를 대상으로 설명해 주세요.
```

```text
Explain it for a non-developer business audience.
```

---

## Step 9: Observe what happened

테스트한 후, 아래 질문에 답해 봅니다.

* agent가 general knowledge를 기반으로 답변했나요, 아니면 Microsoft Learn을 사용했나요?
* documentation을 직접 읽는 것보다 답변이 이해하기 쉬웠나요?

---

# Part 2: Try the prebuilt agent with custom MCP servers

이 Part에서는 이미 생성되어 custom MCP servers에 연결된 agent를 사용합니다.

이 랩에서 사용하는 custom MCP server project는 [GitHub repository](https://github.com/swannekim/mcp-curator-servers)에서 확인할 수 있습니다.

워크숍 중에는 이 server를 직접 배포하거나 구성할 필요가 없습니다. agent는 이미 lab environment에 준비되어 있습니다.

---

## Step 1: Open the prebuilt agent

1. Copilot Studio agent list로 돌아갑니다.
2. 워크숍 진행자가 제공한 prebuilt lab agent를 찾습니다.
3. agent를 엽니다.
4. test chat panel을 엽니다.

---

## Step 2: Copy and Paste Agent Instructions

```text
당신은 Curator MCP Agent입니다. MCP tools로 구동되는 창의적인 museum-style curation assistant입니다. 당신의 역할은 사용자의 요청을 이해하고, 올바른 skill을 선택하며, 적절한 MCP tools를 순서대로 호출하고, polished하지만 compact한 결과를 반환하는 것입니다.

## 사용 가능한 Skills

당신에게는 재사용 가능한 두 가지 skills가 있습니다. 사용자가 명확히 둘 다 요청하지 않는 한, 일반적인 요청에는 정확히 하나의 skill만 사용하세요.

### 1. Museum of the Universe Curator

사용자가 다음 중 하나를 원할 때는 `museum-of-the-universe-curator/SKILL.md`를 사용하세요.

* mini exhibition
* universe museum
* NASA/space imagery와 artworks의 pairing
* loneliness, wonder, chaos, birth, mystery, nostalgia, silence, fear, awe, beauty 같은 단어를 중심으로 한 themed curation
* exhibition map 또는 Mermaid-based museum diagram

일반적인 사용자 표현은 다음과 같습니다.

* “Curate a 5-piece exhibition about loneliness in space.”
* “Make a universe museum about wonder.”
* “Build an exhibition on chaos and cosmic mystery.”

### 2. AI Curator: Art Doppelgänger

사용자가 다음 중 하나를 원할 때는 `art-doppelganger/SKILL.md`를 사용하세요.

* 사용자의 personality와 어울리는 artwork
* “art doppelgänger”
* “what artwork am I?”
* playful personality-to-art match
* 3–5개의 personality words와 optional preferred colours를 기반으로 매칭된 artwork

일반적인 사용자 표현은 다음과 같습니다.

* “Find my art doppelgänger.”
* “I am analytical, romantic, introverted, and secretly dramatic. I like blue and silver.”
* “What artwork am I? Ambitious, chaotic, dramatic.”

## Routing rules

1. 요청이 themed exhibition을 명확히 요구하는 경우, Museum skill을 load하고 따르세요.
2. 요청이 personality-to-artwork match를 명확히 요구하는 경우, Art Doppelgänger skill을 load하고 따르세요.
3. 요청이 exhibition과 personal art match를 모두 언급하는 경우, 명시적으로 요청된 final deliverable을 수행하세요.

   * 사용자가 exhibition을 요청하면, Museum skill을 사용하고 사용자의 personality words를 exhibition theme으로 다루세요.
   * 사용자가 하나의 artwork match를 요청하면, Art Doppelgänger skill을 사용하세요.
   * 사용자가 둘 다 요청하면, Art Doppelgänger를 먼저 실행한 뒤 personality words 또는 best-match mood를 Museum theme으로 사용하세요. 결과는 compact하게 유지하세요.
4. 요청이 모호한 경우, 짧게 한 가지 clarification만 질문하세요: “Do you want a space-art mini exhibition, or your personal art doppelgänger?”
5. 필요한 inputs가 누락된 경우, 선택한 skill에 필요한 최소한의 누락 input만 질문하세요.

## Tools attached

이 MCP tools는 intended purpose에만 사용하세요.

* `NASA Space Data MCP`

  * NASA imagery, APOD, space-theme keywords에만 사용하세요.
  * artworks 또는 Mermaid diagrams에는 사용하지 마세요.

* `Art Institute Public Artwork MCP`

  * public-domain artworks를 검색하거나 artwork metadata를 가져올 때만 사용하세요.
  * images가 있는 artworks를 선호하세요.

* `Curator Scoring MCP`

  * pairing/ranking과 Mermaid syntax 생성을 위해서만 사용하세요.
  * Museum의 경우, `pair_space_images_with_artworks`와 `build_exhibition_mermaid`를 사용하세요.
  * Art Doppelgänger의 경우, `rank_artworks_for_personality`와 `build_doppelganger_mermaid`를 사용하세요.

* `Mermaid Chart Rendering MCP`

  * Curator Scoring MCP가 반환한 complete Mermaid syntax를 validate하고 render할 때만 사용하세요.
  * rendered diagram link를 보관하고 final answer에 포함하세요.

## Global behavior rules

* 톤은 poetic, concise, museum-like하게 유지하세요.
* 모든 matches는 objective truth가 아니라 creative curation으로 다루세요.
* scoring output을 근거로 scientific, psychological, art-historical certainty를 주장하지 마세요.
* usable image URLs가 있는 public-domain artworks를 선호하세요.
* images가 없는 artworks는 제외하세요.
* tool payloads는 compact하게 유지하세요. 선택한 skill에서 지시하지 않는 한 large candidate sets를 가져오지 마세요.
* tool result가 source links를 제공하는 경우, final answer에 항상 source links를 포함하세요.
* Art Doppelgänger skill을 위해 user photos, faces, identity를 절대 분석하지 마세요. 사용자가 제공한 text만 사용하세요.
* Mermaid rendering이 실패하면, Mermaid syntax summary를 plain text로 반환하고 diagram render를 완료할 수 없었다고 간단히 말하세요.
```

``` text
You are Curator MCP Agent, a creative museum-style curation assistant powered by MCP tools. Your job is to understand the user’s request, choose the right skill, call the right MCP tools in order, and return a polished but compact result.

## Skills available

You have two reusable skills. Use exactly one skill for normal requests, unless the user clearly asks for both.

### 1. Museum of the Universe Curator

Use `museum-of-the-universe-curator/SKILL.md` when the user wants any of the following:

* a mini exhibition
* a universe museum
* NASA/space imagery paired with artworks
* a themed curation around words such as loneliness, wonder, chaos, birth, mystery, nostalgia, silence, fear, awe, or beauty
* an exhibition map or Mermaid-based museum diagram

Typical user wording:

* “Curate a 5-piece exhibition about loneliness in space.”
* “Make a universe museum about wonder.”
* “Build an exhibition on chaos and cosmic mystery.”

### 2. AI Curator: Art Doppelgänger

Use `art-doppelganger/SKILL.md` when the user wants any of the following:

* an artwork that matches their personality
* an “art doppelgänger”
* “what artwork am I?”
* a playful personality-to-art match
* an artwork matched from 3–5 personality words and optional preferred colours

Typical user wording:

* “Find my art doppelgänger.”
* “I am analytical, romantic, introverted, and secretly dramatic. I like blue and silver.”
* “What artwork am I? Ambitious, chaotic, dramatic.”

## Routing rules

1. If the request clearly asks for a themed exhibition, load and follow the Museum skill.
2. If the request clearly asks for a personality-to-artwork match, load and follow the Art Doppelgänger skill.
3. If the request mentions both an exhibition and a personal art match, do the explicitly requested final deliverable:

   * If the user asks for an exhibition, use the Museum skill and treat their personality words as the exhibition theme.
   * If the user asks for one artwork match, use the Art Doppelgänger skill.
   * If the user asks for both, run Art Doppelgänger first, then use the personality words or best-match mood as the Museum theme. Keep the result compact.
4. If the request is ambiguous, ask one short clarification: “Do you want a space-art mini exhibition, or your personal art doppelgänger?”
5. If required inputs are missing, ask only for the missing minimum input required by the chosen skill.

## Tools attached

Use these MCP tools only for their intended purpose:

* `NASA Space Data MCP`

  * Use only for NASA imagery, APOD, and space-theme keywords.
  * Do not use it for artworks or Mermaid diagrams.

* `Art Institute Public Artwork MCP`

  * Use only to search public-domain artworks or fetch artwork metadata.
  * Prefer artworks with images.

* `Curator Scoring MCP`

  * Use only for pairing/ranking and generating Mermaid syntax.
  * For Museum, use `pair_space_images_with_artworks` and `build_exhibition_mermaid`.
  * For Art Doppelgänger, use `rank_artworks_for_personality` and `build_doppelganger_mermaid`.

* `Mermaid Chart Rendering MCP`

  * Use only to validate and render complete Mermaid syntax returned by Curator Scoring MCP.
  * Keep the rendered diagram link and include it in the final answer.

## Global behavior rules

* Keep the tone poetic, concise, and museum-like.
* Treat all matches as creative curation, not objective truth.
* Do not claim scientific, psychological, or art-historical certainty from the scoring output.
* Prefer public-domain artworks with usable image URLs.
* Skip artworks without images.
* Keep tool payloads compact. Do not fetch large candidate sets unless the chosen skill says to.
* Always include source links in the final answer when the tool result provides them.
* Never analyze user photos, faces, or identity for the Art Doppelgänger skill. Use only text the user provides.
* If Mermaid rendering fails, still return the Mermaid syntax summary in plain text and briefly say the diagram render could not be completed.
```

---

## Step 3: Upload Agent Skills
이 실습에서 Agent Skills는 이미 업로드가 되어 있습니다. 사용된 Skill.md 파일들은 [GitHub Repository](https://github.com/swannekim/mcp-curator-servers/tree/main/docs/skills)에서 확인할 수 있습니다.

### Copilot Studio의 새로운 Skill 기능 요약

#### 1. Skill이란?

Copilot Studio의 **Skill**은 New Orchestrator가 필요할 때 불러와 사용하는 **재사용 가능한 작업 playbook**입니다.

기존에는 agent의 top-level instructions에 모든 규칙, tool 사용 방식, 예외 처리, workflow를 길게 작성해야 했습니다.
Skill을 사용하면 특정 업무나 시나리오별로 다음 내용을 별도 Markdown 기반 instruction으로 분리할 수 있습니다.

* 언제 이 Skill을 사용할지
* 어떤 tools / MCP servers를 사용할지
* 어떤 순서로 실행할지
* 입력이 부족할 때 어떻게 처리할지
* 최종 답변은 어떤 형식으로 반환할지
* 어떤 guardrails를 지켜야 할지

즉, Skill은 agent에게 “이런 요청이 오면 이 절차대로 처리해”라고 알려주는 **task-specific orchestration guide**로 볼 수 있습니다.

---

#### 2. 왜 중요한가?

이번 Copilot Studio 업데이트의 핵심은 단순 UI 변경이 아니라, agent가 더 복잡한 multi-step task를 처리할 수 있도록 **orchestration 중심 구조**가 강화되었다는 점입니다.

New Orchestrator는 사용자의 요청을 보고 다음을 판단합니다.

* 어떤 Skill이 적절한지
* 어떤 tool이나 MCP server를 호출해야 하는지
* tool 결과를 다음 단계 입력으로 어떻게 넘길지
* 최종적으로 어떤 응답을 만들어야 하는지

따라서 Skill을 잘 작성하면 agent의 동작을 더 일관되게 만들 수 있고, 복잡한 tool chaining도 하나의 재사용 가능한 패턴으로 관리할 수 있습니다.

---

#### 3. Copilot Studio Skill과 기존 구성 요소의 관계

Skill은 Topic, Tool, Connector, MCP server, Knowledge source를 완전히 대체한다기보다는, 이들을 **어떤 상황에서 어떤 순서로 사용할지 정의하는 orchestration layer**에 가깝습니다.

| 구성 요소                     | 역할                                                    |
| ------------------------- | ----------------------------------------------------- |
| Agent instructions        | agent 전체의 기본 역할과 전반적인 행동 규칙                           |
| Skill                     | 특정 업무를 위한 trigger, workflow, tool sequence, guardrail |
| Tool / Connector          | 실제 외부 시스템 호출 또는 action 수행                             |
| MCP Server                | agent가 사용할 수 있는 외부 tools/resources 제공                 |
| Knowledge source          | 답변 grounding을 위한 문서/지식                                |
| Workflow / Power Automate | deterministic process automation이 필요한 절차 실행           |

즉, Skill은 “실행 도구” 자체라기보다는, agent가 여러 tools와 MCP servers를 올바르게 조합해 사용하도록 안내하는 구조입니다.

---

#### 4. 이 Agent에서 사용한 Skill 구조

이번 Curator MCP Agent에서는 두 개의 Skill을 사용했습니다.

1. `museum-of-universe`
2. `art-doppelganger`

두 Skill 모두 공통적으로 다음 구성 요소를 포함합니다.

---

#### 5. Skill 파일의 주요 구성 요소

##### 1. YAML front matter

Skill 파일 상단에는 `name`과 `description`이 있습니다.

```yaml
---
name: museum-of-universe
description: Use this skill when the user wants a 3–5 piece mini exhibition...
---
```

이 부분은 orchestrator가 Skill을 식별하고, 어떤 요청에서 이 Skill을 load해야 하는지 판단하는 데 사용됩니다.

---

##### 2. Skill identity / role

Skill이 어떤 역할을 수행하는지 정의합니다.

예를 들어 `Museum of the Universe Curator`는 사용자의 theme을 받아 NASA space imagery와 public-domain artworks를 pairing하여 mini exhibition을 만드는 역할을 합니다.

`AI Curator: Art Doppelgänger`는 사용자가 제공한 personality words와 preferred colours를 기반으로 Art Institute of Chicago collection에서 어울리는 artwork를 찾아주는 역할을 합니다.

---

##### 3. When to use this skill

이 Skill을 언제 사용할지 정의하는 trigger 영역입니다.

예를 들어 `museum-of-universe` Skill은 다음 요청에 사용됩니다.

* mini exhibition
* universe museum
* NASA/space imagery paired with artworks
* themed art-and-space curation
* exhibition map 또는 Mermaid diagram

반면 `art-doppelganger` Skill은 다음 요청에 사용됩니다.

* art doppelgänger
* “what artwork am I?”
* personality-to-artwork match
* “my vibe as a painting”

이 부분이 명확해야 orchestrator가 잘못된 Skill을 선택하지 않습니다.

---

##### 4. Required tools

Skill이 사용할 MCP tools와 호출 순서를 정의합니다.

`museum-of-universe` Skill은 다음 순서로 tools를 사용합니다.

1. `NASA Space Data MCP`
2. `Art Institute Public Artwork MCP`
3. `Curator Scoring MCP`
4. `Mermaid Chart Rendering MCP`

`art-doppelganger` Skill은 NASA tool을 사용하지 않고, artwork search, personality ranking, Mermaid rendering 중심으로 tools를 사용합니다.

이처럼 Skill 안에 tool sequence를 명시하면 multi-step tool chaining이 더 안정적으로 동작합니다.

---

##### 5. Input handling

사용자의 입력이 부족하거나 불완전할 때 어떻게 처리할지 정의합니다.

예를 들어 `museum-of-universe` Skill은 사용자가 theme을 주지 않으면 짧은 theme 하나만 요청하고, 작품 수를 지정하지 않으면 기본값으로 5개를 사용합니다.

`art-doppelganger` Skill은 사용자가 3–5개의 personality words를 주지 않으면 이를 요청하고, 문장으로 입력한 경우 agent가 직접 3–5개의 personality words를 추출하도록 합니다.

---

##### 6. Process

Skill의 핵심 workflow입니다.

여기에는 agent가 어떤 순서로 reasoning하고 tools를 호출해야 하는지가 단계별로 정의됩니다.

예를 들어 `museum-of-universe` Skill의 process는 다음과 같습니다.

1. exhibition theme 식별
2. NASA image 검색
3. Art Institute artwork 검색
4. image가 없는 artwork 제거
5. space image와 artwork pairing
6. exhibition title, curator statement, wall label 생성
7. Mermaid exhibition map 생성
8. Mermaid rendering 수행

이 구조 덕분에 agent는 매번 비슷한 품질과 순서로 결과를 생성할 수 있습니다.

---

##### 7. Final response format

최종 응답 형식을 정의합니다.

`museum-of-universe` Skill은 다음을 포함하도록 설계되었습니다.

* exhibition title
* curator statement
* NASA image와 artwork pairing list
* wall label
* match score
* source links
* rendered exhibition-map link

`art-doppelganger` Skill은 다음을 포함합니다.

* best-match artwork
* match score
* personality/color 기반 explanation
* runner-ups
* rendered diagram link
* artwork source links

이 부분은 live demo에서 결과를 보기 좋게 만들고, agent 응답의 일관성을 높여줍니다.

---

##### 8. Style rules

응답 톤과 표현 방식을 정의합니다.

이번 agent에서는 museum-style demo에 맞게 다음과 같은 스타일을 사용했습니다.

* poetic
* concise
* museum-like
* playful but elegant
* creative curation으로 표현
* scoring을 객관적 진실처럼 말하지 않기

특히 artwork match나 exhibition pairing은 “scientific result”가 아니라 creative curation으로 다루도록 guardrail을 두었습니다.

---

##### 9. Error handling

tool 결과가 부족하거나 rendering이 실패했을 때의 fallback behavior를 정의합니다.

예를 들어:

* NASA images가 부족하면 가능한 image만으로 exhibition size 축소
* artwork search 결과가 부족하면 mood term을 한 번 broaden
* Mermaid rendering이 실패하면 diagram link 없이 결과 제공
* source link가 없으면 tool이 제공한 link만 포함

이 부분은 demo 중 tool 결과가 예상과 달라도 agent가 자연스럽게 대응하도록 돕습니다.

---

#### 6. 이번 Curator MCP Agent에서의 Skill 설계 의미

이번 agent는 하나의 top-level agent가 두 가지 Skill을 route하는 구조입니다.

| Skill                | 사용 목적                                               | 주요 MCP tools                                                                   |
| -------------------- | --------------------------------------------------- | ------------------------------------------------------------------------------ |
| `museum-of-universe` | space imagery와 artwork를 pairing한 mini exhibition 생성 | NASA Space Data MCP, Art Institute MCP, Curator Scoring MCP, Mermaid Chart MCP |
| `art-doppelganger`   | personality words 기반 artwork match                  | Art Institute MCP, Curator Scoring MCP, Mermaid Chart MCP                      |

Agent의 top-level instruction은 사용자의 요청을 보고 어떤 Skill을 사용할지 판단합니다.
각 Skill은 실제 업무 처리 방식, tool 호출 순서, 응답 형식, 예외 처리를 담당합니다.

즉, 전체 구조는 다음과 같이 이해할 수 있습니다.

```text
User request
  ↓
Top-level Agent Instructions
  ↓
Skill Routing
  ↓
Selected Skill
  ↓
MCP Tool Chaining
  ↓
Final curated response
```

---

## Step 4: Register the 4 MCP servers as tools

**각** server에 대해 다음 순서로 진행합니다. **Tools → Add a tool → New tool → Model Context Protocol**

| Server name         | Server URL                                                                            | Transport  | Auth |
| ------------------- | ------------------------------------------------------------------------------------- | ---------- | ---- |
| NASA Data MCP       | `https://mcp-curator-nasa-sage.kindmoss-321119b4.eastus.azurecontainerapps.io/mcp`    | Streamable | None |
| Art Institute MCP   | `https://mcp-curator-artic-sage.kindmoss-321119b4.eastus.azurecontainerapps.io/mcp`   | Streamable | None |
| Curator Scoring MCP | `https://mcp-curator-scoring-sage.kindmoss-321119b4.eastus.azurecontainerapps.io/mcp` | Streamable | None |
| Mermaid Chart MCP   | `https://mcp.mermaid.ai/mcp`                                                          | Streamable | None |

**Create**를 클릭합니다. Copilot Studio가 custom connector를 생성합니다. **create a connection** 메시지가 표시되면,
진행합니다. `"None"` auth인 경우 한 번 클릭하면 됩니다. 그러면 server의 tools가 표시됩니다. 예: `get_space_images_for_theme`,
`find_artworks_by_mood`, `pair_space_images_with_artworks` 등

**Server descriptions** (붙여넣기 — agent가 잘못된 server를 호출하지 않도록 도와줍니다):

- **NASA Data:**
    ```text
    Use only to retrieve NASA space imagery, astronomy descriptions, and space keywords. Not for artwork or diagrams.
    ```
    - NASA space imagery, astronomy descriptions, space keywords를 가져올 때만 사용하세요. artwork나 diagrams에는 사용하지 마세요.
- **Art Institute:**
    ```text
    Use only to search public-domain artworks and retrieve artwork metadata and IIIF image URLs.
    ```
    - public-domain artworks를 검색하고 artwork metadata와 IIIF image URLs를 가져올 때만 사용하세요.
- **Curator Scoring:**
    ```text
    Use only to pair space images with artworks, rank artworks for a personality, and generate Mermaid diagram syntax. Does no external lookups.
    ```
    - space images와 artworks를 매칭하고, personality에 맞는 artworks를 ranking하며, Mermaid diagram syntax를 생성할 때만 사용하세요. 외부 조회는 수행하지 않습니다.
- **Mermaid Chart:**
    ```text
    Use only to validate and render Mermaid diagram syntax into a PNG/SVG/playground link.
    ```
    - Mermaid diagram syntax를 validate하고 PNG/SVG/playground link로 render할 때만 사용하세요.

---

## Step 5: Try example prompts

아래 예시 prompts를 사용합니다.

### Prompts for Universe Exhibition Curator

#### Prompt 1
```text
우주 속 외로움에 대한 5-piece exhibition을 curate해 주세요.
```
```text
Curate a 5-piece exhibition about loneliness in space.
```
#### Prompt 2
```text
wonder를 주제로 universe museum을 만들어 주세요.
```
```text
Make a universe museum about wonder.
```
#### Prompt 3
```text
chaos와 cosmic mystery에 대한 exhibition을 만들어 주세요.
```
```text
Build an exhibition on chaos and cosmic mystery.
```

### Prompts for Art Doppelganger Curator

#### Prompt 4
```text
내 art doppelganger를 찾아 주세요. 나는 analytical, romantic, introverted하고, secretly dramatic합니다. blue와 silver를 좋아합니다.
```
```text
Find my art doppelganger. I am analytical, romantic, introverted, and secretly dramatic. I like blue and silver.
```
#### Prompt 5
```text
나는 calm, curious, nostalgic합니다. 나와 어울리는 artwork를 매칭해 주세요. green과 gold를 좋아합니다.
```
```text
I am calm, curious, and nostalgic. Match me with an artwork. I like green and gold.
```
#### Prompt 6
```text
나는 어떤 artwork인가요? Ambitious, chaotic, dramatic.
```
```text
What artwork am I? Ambitious, chaotic, dramatic.
```

---

## Step 6: Discuss what is different

두 agents를 비교해 봅니다.

| Agent                     | What it connects to        | What participants should notice                       |
| ------------------------- | -------------------------- | ----------------------------------------------------- |
| Microsoft Learn Guide     | Microsoft Learn MCP Server | official Microsoft documentation과 code samples를 사용합니다 |
| Prebuilt custom MCP agent | Custom MCP servers         | custom tools와 curated resources 또는 workflows를 사용합니다   |

핵심 takeaway는 다음과 같습니다.

> MCP를 사용하면 Copilot Studio agents가 모든 답변을 agent 내부에 hard-code하지 않고도 external tools와 knowledge sources를 사용할 수 있습니다.

---

# Concepts to remember

## 1. MCP is a connection standard

MCP는 agents가 external tools, resources, prompts에 일관된 방식으로 연결할 수 있게 해 줍니다.

## 2. The agent decides when to use a tool

agent는 사용자의 질문을 읽고 MCP tool이 유용한지 판단합니다.

## 3. Tool descriptions matter

명확한 server와 tool descriptions는 agent가 각 tool을 언제 사용해야 하는지 이해하는 데 도움이 됩니다.

## 4. Copilot Studio supports Streamable HTTP for MCP

Copilot Studio에서 MCP servers는 지원되는 Streamable HTTP transport를 사용해야 합니다.

## 5. MCP can connect to Microsoft and custom sources

이 랩에서는 다음 두 가지를 모두 확인했습니다.

* Microsoft Learn MCP Server
* 워크숍을 위해 준비된 Custom MCP servers

---

# Wrap-up

이 랩에서는 Copilot Studio agent를 만들고, Microsoft Learn MCP Server에 연결한 뒤, agent가 신뢰할 수 있는 external documentation을 어떻게 사용할 수 있는지 테스트했습니다.

또한 custom MCP servers에 연결된 prebuilt agent도 사용해 보았습니다.

> MCP는 Copilot Studio agents를 신뢰할 수 있는 tools와 knowledge sources에 연결함으로써 agents가 더 유용해지도록 도와줍니다.

---

# References

* Microsoft Learn MCP Server overview: https://learn.microsoft.com/en-us/training/support/mcp
* Microsoft Learn MCP Server developer reference: https://learn.microsoft.com/en-us/training/support/mcp-developer-reference
* Connect a Copilot Studio agent to an MCP server: https://learn.microsoft.com/en-us/microsoft-copilot-studio/mcp-add-existing-server-to-agent
* Extend a Copilot Studio agent with MCP: https://learn.microsoft.com/en-us/microsoft-copilot-studio/agent-extend-action-mcp
* Microsoft Learn MCP Server GitHub repository: https://github.com/microsoftdocs/mcp
* Custom lab MCP server repository: https://github.com/swannekim/mcp-curator-servers/tree/main
