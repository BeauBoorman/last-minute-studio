# Agentic Cinema — hard constraints

Verified against the published rules. Any one of these is pass/fail, so they belong in the
repo rather than in a plan document.

**Deadline:** 2026-09-07, 14:00 PDT.

## Mandatory

- **Runtime AI must be Google Cloud (Gemini / Vertex) + the chosen partner only.**
- The repo must **actually import and call** Google packages at runtime (`google-adk`,
  `google-genai`) — proven in code, not name-dropped in a README.
- Must integrate **a Partner Entity's MCP server** (see open questions — this one is unclear).
- **Public, cloneable repo with a visible license**, secret-scanned before the first commit.
- **IBM track:** use IBM Bob in the development process from commit #1.

## Prohibited

> "No other AI models, agent frameworks, or AI APIs are permitted, regardless of vendor —
> this includes but is not limited to AWS, Microsoft, OpenAI, and Anthropic AI tools."

Practical consequences for this repo:

- **No non-Google TTS or music generation** in the submitted runtime. A "Voice Agent" or
  "Music Agent" must call Google Cloud services, not ElevenLabs or similar.
- No OpenAI / Anthropic / local-GPU inference in the submitted runtime. Local hardware is
  fine for *internal testing*, never in the judged path.
- Third-party SaaS that uses AI internally is an open question (see below).

## Newly-created-project rule

> "Projects must be newly created by the entrant during the Contest Period. The Project must
> be Your original creation not a modification or extension of Your or anyone else's
> existing work."

`calesthio/OpenMontage` is **AGPL-3.0** (verified first-party via the GitHub API and the raw
LICENSE file). It is treated as **inspiration only** — not forked, not vendored. Two reasons:
the new-project rule above, and AGPL copyleft, which would force this repo off MIT and
require any hosted service to publish its full source.

## Suggested guardrail

Pin the model in code with a fail-fast allowlist, enforced at import time so a
misconfiguration cannot reach the judged runtime:

```python
# src/config/model_whitelist.py
ALLOWED_MODELS = {"gemini-2.5-flash-lite"}   # sys.exit(2) on anything else
```

A local constant checked at import needs no network call at boot, so an outage or an offline
test can never crash startup — and no non-compliant model can be reached by accident.
