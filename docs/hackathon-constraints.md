# Agentic Cinema — constraints

Official rules: <https://agentic-cinema.devpost.com/rules>

This file separates **contest requirements** (their rules — breaking one is pass/fail) from
**our engineering guardrails** (our choices — sensible, but not imposed by the contest). The
distinction matters: a rule is non-negotiable, a guardrail is a decision we can revisit.

**Deadline:** 2026-09-07, 14:00 PDT.

---

## A. Contest requirements — quoted from the rules

**Required stack.** Build *"a functional, production-ready AI agent or multi-agent network —
powered by Gemini and Google Cloud Agent Builder"*, and integrate *"a Partner Entity's MCP
server."*

**Prohibited AI.**
> "No other AI models, agent frameworks, or AI APIs are permitted, regardless of vendor —
> this includes but is not limited to AWS, Microsoft, OpenAI, and Anthropic AI tools."

Practical consequences for this repo:
- No non-Google TTS or music generation in the submitted runtime — narration and any audio
  generation must call Google Cloud services.
- No OpenAI / Anthropic / local-GPU inference in the judged path. Local hardware is fine for
  internal testing, never in the submitted runtime.
- Whether third-party SaaS that uses AI *internally* counts is unresolved — see
  [open questions](open-questions-organizers.md).

**Newly created project.**
> "Projects must be newly created by the entrant during the Contest Period. The Project must
> be Your original creation not a modification or extension of Your or anyone else's existing
> work."

**Submission.** A hosted project URL, a 3-minute demo video, and a **public open-source
repository with a visible license file**. (This repo is public and MIT — requirement met.)

**Eligibility.** Legal age of majority; 23 excluded countries/territories.

---

## B. Our engineering guardrails — our decisions, not contest rules

These are ours. They are here because they are good practice, not because the contest demands
them. Argue with any of them.

**Model pinning.** Pin the model in code with a fail-fast allowlist enforced at import time:

```python
# src/config/model_whitelist.py
ALLOWED_MODELS = {"gemini-2.5-flash-lite"}   # sys.exit(2) on anything else
```

*Why:* the prohibited-AI rule above is pass/fail, and the cheapest way to guarantee compliance
is to make a non-compliant model ID impossible to reach at runtime. A local constant checked at
import needs no network call at boot, so an outage can never crash startup.

**Secret scanning.** Run `gitleaks` in CI and as a pre-commit hook. *Not a contest requirement.*
It is here because this is a public repo and a leaked key is unrecoverable once pushed. The
earlier draft of this file said "before the first commit," which is no longer actionable for an
existing repository — the practical version is: **scan every commit from now on, and scan the
existing history once** to confirm nothing is already exposed.

**Dependency posture.** `calesthio/OpenMontage` is **AGPL-3.0** (verified via the GitHub API and
its raw LICENSE file). We treat it as **inspiration only** — not forked, not vendored. Two
reasons: the newly-created-project rule above, and AGPL copyleft, which would force this repo
off MIT and require any hosted deployment to publish its full source. Whether it could be used
as a plain licensed dependency is an [open question](open-questions-organizers.md) for the
organizers, not something to assume either way.
