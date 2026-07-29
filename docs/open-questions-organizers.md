# Organizer questions — Agentic Cinema (Stage-One DQ risks)

**For:** Timothy to post on the Devpost discussion thread / organizer contact.
**Why it is urgent:** `plan-260728-studiodesk-hackathon-master.md` §11 records the thread as
**unsent**, and states *"Architecture stays flexible until answers return."* These three
answers unblock the architecture decision. Nothing downstream should be locked before them.
**Deadline context:** submissions close **2026-09-07 14:00 PDT**.

Post as three separate questions — organizers answer short, specific questions far more
reliably than one long one. Each quotes the rule it is about, so the answer is unambiguous.

---

## Q1 — Where is the line between "not an extension of existing work" and a licensed dependency?

> Your rules state: *"Projects must be newly created by the entrant during the Contest Period.
> The Project must be Your original creation not a modification or extension of Your or anyone
> else's existing work."*

We are building a **new** application on Gemini and Google Cloud Agent Builder. We are not
forking or submitting any existing project.

Our question is about ordinary open-source dependencies. May we depend on an existing
open-source library for **deterministic, non-AI media operations only** — for example
FFmpeg orchestration or a React/Remotion render layer — where that library is installed as a
dependency, its AI provider integrations are unused, and all agent logic and orchestration
are written by us during the Contest Period?

Specifically: does "not a modification or extension of existing work" restrict **dependencies**,
or only the submitted project itself? If dependencies are permitted, is an **AGPL-3.0**
licensed dependency acceptable given the requirement for a public repo with a visible license?

## Q2 — How is the "Partner Entity's MCP server" requirement satisfied on the IBM track?

> The challenge requires integrating *"a Partner Entity's MCP server."*
> The IBM track package directs entrants to use **IBM Bob** during development.

IBM's own documentation describes Bob as an MCP **client**, with no preinstalled MCP servers
(https://bob.ibm.com/docs/ide/configuration/mcp/mcp-in-bob). A client and a server are not
interchangeable, so we cannot tell which artifact satisfies the requirement.

Which of these satisfies it?
1. Using IBM Bob (an MCP client) during development;
2. Connecting our agent to a specific IBM-hosted MCP **server** — if so, which one, and where
   is its endpoint documented;
3. Building and hosting our own MCP server that exposes our tools to a partner product.

## Q3 — Does the AI-vendor restriction cover third-party SaaS that uses AI internally, and does it cover build-time tooling?

> Your rules state: *"No other AI models, agent frameworks, or AI APIs are permitted,
> regardless of vendor — this includes but is not limited to AWS, Microsoft, OpenAI, and
> Anthropic AI tools."*

We read this as governing the **submitted application's runtime**, and we intend to run all
runtime inference on Gemini / Google Cloud only. Two boundaries are unclear:

**(a) Production SaaS with AI inside.** Does the restriction extend to third-party media tools
that happen to use AI internally (for example a hosted video editor or a non-Google
text-to-speech vendor) when they are used as production tooling rather than as the agent's
reasoning model? If such tools are disallowed, we will use Google Cloud Text-to-Speech and
equivalent Google services throughout.

**(b) Build-time tooling.** Does the restriction apply to **development** tools — e.g. an AI
coding assistant used to write source code — or only to what the submitted application invokes
at runtime?

---

## What each answer changes

| Answer | Consequence |
|---|---|
| Q1 — dependencies allowed | Deterministic render/media layer can be reused; large schedule saving |
| Q1 — dependencies restricted | Every media operation is written from scratch; scope must shrink now |
| Q2 — Bob-as-client suffices | No extra infrastructure |
| Q2 — a real MCP server is required | New build item, must be scheduled immediately |
| Q3a — SaaS-with-AI disallowed | All TTS/editing routes to Google Cloud equivalents |
| Q3b — build-time tooling restricted | The agent build fleet itself has to change |

**Until Q1 and Q2 return, do not commit to an architecture that assumes either answer.**

> The IBM Bob client/server contradiction is quoted from our OpenMontage teardown, which cites
> IBM's documentation. Worth re-checking against IBM's docs before posting.
