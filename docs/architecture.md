# Architecture

Last Minute Studio is designed as a sequence of small, inspectable agents connected by explicit artifacts. Each stage should be retryable and should leave behind enough structured output for a human to review or resume a job.

## Pipeline

> **Direction change (2026-07-29).** This project is an *editorial assembly* system, not an
> AI-video generator. See [`concept-studiodesk.md`](concept-studiodesk.md) for the reasoning.
> The seven-stage generative pipeline previously described here is superseded.

**Three agents, plus deterministic tools.**

1. **Ingest & Match Agent** — script/brief + an indexed catalog of source material
   (repo evidence, screen recordings, licensed clips, transcripts) → a **Timeline AST**.
2. **Compliance Agent** — rights, licensing and policy gate over every asset before it is used.
3. **Assembly Agent** — Timeline AST → **FCPXML v1.10** *and* a local **FFmpeg preview MP4**.

Everything else in the pipeline is a **tool**, not an agent: repository scraping, transcript
indexing, thumbnailing, FFmpeg calls, FCPXML serialisation.

### Why three, and why tools for the rest

This is the part worth internalising, because it generalises well beyond this project:

- **Agents are for judgement; tools are for work.** If a step has one correct answer given its
  input — serialise this AST to FCPXML, extract this frame — it must be a tool. Tools are
  deterministic, unit-testable, cheap, and cannot hallucinate. Every step you promote to an
  agent is a step that can invent something.
- **Cost scales with agent count**, and our credit budget is capped at $100 with no guarantee.
- **Coherence is scored, surface area is not.** Judging rewards "a complete, coherent product
  experience," which three well-joined agents demonstrate better than seven thin ones.

## Product principle: hands-off by default, hands-on by choice

**The pipeline must produce a good result with zero human input.** Approval steps and the
FCPXML export are *escape hatches for people who want them*, never required steps. If a user
has to babysit it, the product has failed on its own terms.

That creates the hardest engineering requirement in this project: **quality without a human in
the loop.** Two things make it achievable rather than aspirational.

### 1. An automated quality gate (not an agent — a checkable contract)

Before anything is exported, the cut is checked against measurable thresholds:

```text
duration        within target window
captions        legible: contrast, safe-area, min on-screen dwell
audio           narration peak/LUFS in range; music ducked under speech
assets          no missing/expired/placeholder media
coverage        every script line mapped to real source material
claims          every stated fact traceable to repository evidence
```

Each is a **pass/fail check with a number**, so failures are specific and repairable — a stage
can be re-run rather than the whole job. This is the mechanism that makes "hands-off" safe: the
system knows when it produced something bad, instead of hoping a human notices.

*(This is Timothy's Quality Review stage from the original design. It was dropped when the
pipeline collapsed to three agents — restoring it as a deterministic gate is what makes the
hands-off promise credible.)*

### 2. Music and narration are generated, and that is not a contradiction

Assembling real footage rather than generating imagery is the strategic bet. **Generated audio
is not in tension with it** — working editors have always scored cuts with library or
commissioned music. The "AI slop" risk lives in fabricated *imagery*, not in a soundtrack.

So: **narration and music are generated, to a quality bar, with no manual step.**
Google's **Lyria** models are the compliant route (Vertex AI, documented under the Gemini
Enterprise Agent Platform — the platform the contest requires). Lyria 3 Pro produces structured
compositions up to three minutes with real intro/verse/chorus structure, which is what makes an
automatic score sound intentional rather than looped.

The soundtrack is treated as a **tool call with a ducking plan derived from the narration
timing**, not as an agent decision. Deterministic, testable, and hands-off.

*Caveat worth tracking:* Lyria is in public preview. Any preview surface can change or
rate-limit, so the assembly path needs a fallback that still ships a watchable cut.

## Core artifacts

```text
SourceCatalog  indexed material: clips, screen recordings, transcripts, repo evidence
Brief          the claim being made and the story that carries it
TimelineAST    ordered, typed edit decisions — the single source of truth for a cut
RightsReport   per-asset licence status and clearance decisions
Exports        FCPXML v1.10 + FFmpeg preview MP4
```

Keeping these as JSON means each stage is independently testable and a job can resume after a
failure instead of restarting. The **TimelineAST is the important one** — because the edit is
data rather than a rendered file, the same cut can be exported to an editor *or* previewed as
video without re-running anything upstream.

## Design principles

- Evidence before invention: claims should be traceable to repository content or user-provided assets.
- Human approval at high-leverage points: approve the project brief and storyboard before expensive rendering.
- Provider-agnostic media adapters: voice, music, and rendering providers should be replaceable.
- Deterministic reruns: preserve inputs, prompt versions, and provider settings for every job.

> The design principles above predate the direction change and were kept
> deliberately — they hold under either product, and they match what our teardown of
> OpenMontage identified as its strongest ideas.
