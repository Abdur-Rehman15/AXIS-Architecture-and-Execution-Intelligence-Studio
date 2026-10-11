# A.X.I.S. — Architecture & Execution Intelligence Studio

**Category:** Intelligent App Creation
**Team:** Ahmed Nasir, Baber Ali, Abdur Rehman — BS Computer Science, FAST-NUCES Lahore
**Supervisor:** Dr. Mirza Mubasher Baig

> A "company brain" that turns a software team's scattered decisions into a live knowledge graph, and makes AI coding agents follow them.

---

## 1. Project Overview & Problem Statement

AI coding agents such as **Claude Code, Cursor and Antigravity** now write a large share of production code. They are excellent at writing code that works, but they have no idea *what the team has decided*.

In a real software team, the rules of a project live in many places:

- a Slack thread where the team lead decided to use Stripe instead of JazzCash,
- a pull request review saying the database layer must never be accessed from the frontend,
- an SRS document from the client,
- and the heads of senior developers.

The coding agent sees none of this. As a result it:

- **breaks architectural boundaries** (architectural drift),
- **uses outdated decisions** that the team has already reversed,
- **builds features out of the planned order**,

and senior developers must manually review AI-generated code against decisions that are written nowhere in one place. This causes rework, technical debt and slower delivery, which is the opposite of what AI coding tools promise.

**Problem in one line:** AI coding agents are fast, but disconnected from the team's knowledge, so their speed creates rework instead of saving time.

## 2. Proposed Solution

A.X.I.S. is an intelligent desktop application and MCP server that sits between a team's knowledge and its AI coding agents.

1. **Collect** – It connects to Slack channels, GitHub repositories and uploaded SRS documents, and lets the technical lead draw the intended architecture and development order on a visual canvas.
2. **Understand** – A Large Language Model extracts requirements, decisions and constraints from these sources. A consolidation step compares new decisions with existing ones and marks outdated ones as **superseded**, so only the latest decision is ever used.
3. **Connect** – Everything is stored in a **traceability graph** in which every rule is linked to the Slack message, pull request or document it came from.
4. **Deliver** – Through the open **Model Context Protocol (MCP)**, any compatible coding agent receives the relevant, up-to-date context before it writes code. Developers can also ask the graph questions such as *"Who implemented the payment feature?"* or *"Why do we use Stripe?"*.
5. **Guard** – A semantic linter checks the developer's request against active constraints. If a violation is found, the developer gets a **yes/no confirmation inside their own coding agent** (Human-in-the-Loop). The developer stays in control, and every override is recorded.

### Example

> The team decided on Slack (12 Sep) to switch from JazzCash to Stripe.
> A developer later asks Claude Code: *"Integrate JazzCash in the checkout page."*
>
> A.X.I.S. finds the active decision, and the agent responds:
> *"This conflicts with decision DEC-31: the team switched to Stripe (Slack #payments, 12 Sep, Team Lead). Do you want to proceed?"*
>
> **No** → the agent uses Stripe instead. **Yes** → the agent proceeds and the override is logged with its reason.

### What makes it different

| Existing tool | What it does | What it lacks |
|---|---|---|
| Atlassian Rovo | Gives agents organizational context via MCP | Models general work items, not architectural rules; no violation checks; Atlassian-only |
| Kiro (AWS) | Spec-driven agentic development | Specs come from one developer's prompt, not team decisions; they go stale |
| Tessl | Reusable agent "skills" across agents | Skills are written manually; static instructions |
| Jama Connect | Live requirement traceability | Requires formal manual entry; ignores Slack/GitHub discussions |
| Structurizr / Eraser.io | Architecture as code / diagrams | Static documentation; no enforcement on AI agents |

**No existing tool automatically captures informal decisions from team communication *and* checks AI agent requests against them before code is generated.** This is the gap A.X.I.S. fills.

## 3. Objectives & Expected Outcomes

**Objectives**

- Extract requirements, decisions and constraints automatically from Slack, GitHub and documents.
- Keep the knowledge current by superseding outdated decisions.
- Link every rule to its original source for full traceability.
- Deliver relevant context to coding agents through a single, vendor-neutral MCP server.
- Detect constraint violations and confirm them with the developer before code is written.
- Work across multiple coding agents instead of being tied to one vendor.

**Expected outcomes (measurable)**

| Outcome | Target |
|---|---|
| Context and constraint check returned to the coding agent | within 3 seconds for at least 95% of requests |
| New Slack and GitHub activity reflected in the knowledge graph | within 5 minutes |
| Supported coding agents | Claude Code, Cursor and Antigravity |
| Developer effort to connect a coding agent | one configuration step (API key) |
| Behaviour if A.X.I.S. is down | the agent keeps working normally (fail-safe) |
| Hardware | runs on standard laptops, no GPU required |

Extraction accuracy and violation detection (precision and recall) will be measured on a prepared test project containing known decisions, reversed decisions and violating prompts.

## 4. Target Users

| User | Role in A.X.I.S. |
|---|---|
| **Technical Leads / Architects** | Define the architecture, connect Slack and GitHub, make the decisions A.X.I.S. enforces |
| **Project Managers** | Define the development order, upload SRS documents, manage developer access |
| **Software Developers** | Use their usual coding agent; receive context and Yes/No alerts without leaving it |
| **Software Houses** | Deliver client projects with fewer architectural errors and less rework |

The primary market is software houses and product teams that have adopted AI coding agents, a rapidly growing segment.

## 5. Key Features / Deliverables

**Features**

- Visual **structural architecture canvas** (components and allowed dependencies)
- Visual **developmental architecture canvas** (stages and order of development)
- **Slack and GitHub integration** via OAuth and webhooks, with channel/repository selection
- **SRS document upload** (PDF, DOCX, Markdown) with automatic requirement extraction
- **Traceability graph** with superseding of outdated decisions and source links
- **MCP server** with API-key access for developers' coding agents
- **Hybrid retrieval**: keyword search + vector search + graph traversal
- **Semantic architecture linter** with **Human-in-the-Loop** yes/no confirmation
- **Question answering** over project knowledge ("who implemented…", "why did we…")
- **Decision and override log** for full auditability

**Deliverables**

1. A.X.I.S. standalone desktop application (Windows, macOS, Linux)
2. A.X.I.S. MCP server connectable to Claude Code, Cursor and Antigravity
3. A demo project with sample Slack, GitHub and SRS data showing the full flow
4. User manual and technical documentation

## 6. Implementation Plan & Timeline (12 weeks)

**Current status:** Requirements specification and system design are complete (use cases, architecture, database design, class and sequence diagrams, UI screen designs).

| Phase | Weeks | Work | Milestone |
|---|---|---|---|
| 1. Foundation | 1–2 | Database schema on Supabase, authentication, projects, API keys, desktop app shell | Users can sign in, create projects and generate API keys |
| 2. Architecture canvas | 3–4 | Structural and developmental canvases; conversion of the canvas into constraints | Architecture drawn on canvas is stored as rules |
| 3. Knowledge engine | 4–6 | Document upload, Slack and GitHub ingestion, LLM extraction, consolidation and superseding, embeddings | Decisions from Slack, GitHub and SRS appear in the graph with sources |
| 4. MCP server & retrieval | 7–8 | MCP tools, hybrid retrieval with graph expansion, question answering, API-key verification | Claude Code receives correct project context |
| 5. Linter & HITL | 9–10 | Constraint checking, yes/no confirmation in the agent, override logging, Cursor and Antigravity support | Violating prompt triggers a confirmation in all three agents |
| 6. Evaluation & demo | 11–12 | Testing on the prepared project, performance tuning, documentation, demo video | Measured results and a complete end-to-end demo |

Phases 2 and 3 overlap in week 4 because they are developed in parallel by different team members.

## 7. Resources or Requirements

| Type | Details |
|---|---|
| **Team** | 3 developers (front-end, RAG back-end, MCP back-end) and a faculty supervisor |
| **Hardware** | Standard laptops (16 GB RAM, multi-core CPU); no GPU needed |
| **Front-end** | React, React Flow, Tauri (desktop packaging) |
| **Back-end** | Python, FastAPI, FastMCP |
| **Database** | Supabase (PostgreSQL + pgvector), Supabase Auth |
| **AI** | Gemini API for extraction and checking (provider configurable); fastembed for local CPU embeddings |
| **Integrations** | Slack API, GitHub App and webhooks, Model Context Protocol |
| **Needed support** | LLM API credits and cloud hosting for the demo; mentorship from industry on real team workflows |

## 8. Potential Challenges or Risks

| Risk | Mitigation |
|---|---|
| LLM misreads a decision or a request (false alerts or missed violations) | Structural rules checked deterministically; LLM used only for fuzzy cases; the developer always makes the final decision |
| Added latency in the coding agent | Fast hybrid retrieval, small context packs, caching; 3-second target |
| Coding agents implement MCP differently | Built on the open MCP standard; yes/no via MCP elicitation where supported and via chat otherwise; tested on all three agents |
| Slack/GitHub rate limits | Event-driven webhooks instead of polling; automatic retries; periodic re-sync |
| Privacy of team conversations | Read-only OAuth access, only selected channels/repositories, per-organization data isolation, encrypted tokens |
| Decisions made verbally are never captured | Canvas and SRS upload let the team record them directly |
| No real enterprise data for testing | Prepared realistic sample project with known decisions and violations |

## 9. Additional Details

- **Why this is an AI project:** A.X.I.S. uses LLMs for knowledge extraction and semantic validation, retrieval-augmented generation over a knowledge graph for context, and integrates directly with agentic AI tools through MCP.
- **Responsible AI:** A.X.I.S. never silently blocks or changes a developer's work. A human confirms every conflict, and every decision is traceable to its source.
- **Extensibility:** Each knowledge source is a separate connector, so Jira, Confluence or other tools can be added later without changing the core system.
- **SDG alignment:** SDG 9 (Industry, Innovation and Infrastructure): making AI-assisted software development more reliable for software organizations.
- **Project status:** A.X.I.S. is our Final Year Project at FAST-NUCES Lahore, with requirements and design documentation already completed.
