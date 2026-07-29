# Concept proposal — editorial assembly desk (alternative direction)

**Status:** proposal for discussion. This is **not** a correction of `architecture.md`, and it
should not be merged as a silent replacement. It is a genuinely different product bet that
came out of a week of research, and it needs an explicit decision before either direction
gets built.

## The fork, stated plainly

| | **Current README** | **This proposal** |
|---|---|---|
| What it is | AI video **generator** | Editorial **assembly desk** |
| Input | repo + screenshots | licensed footage catalog + script/brief |
| AI's job | write, narrate, score, render | index, match, clear rights, assemble |
| Output | finished **MP4** | native **FCPXML** + low-res preview |
| Finished by | nobody — it's done | a human editor, in Final Cut |
| Main risk | reads as "AI slop" | needs a footage catalog + an editor |

## Why the research landed here

- **"AI slop" is a scoring risk.** Judging weights *Design* as "a complete, coherent product
  experience beyond proof-of-concept." A fully generated 60-second video is the single most
  common hackathon submission in this category, and it invites comparison against Veo-class
  output we cannot match under the Google-only constraint.
- **It plays to a real craft.** Orlando is an editor. A tool that hands a working editor a
  clean FCPXML is a real studio workflow; a tool that replaces them is a demo.
- **Rendering is the expensive part.** Cutting cloud rendering takes GCP render spend to zero,
  which matters while credit amounts are unknown.
- **Rights clearance is a differentiator.** A compliance gate over licensed footage is an
  obvious, legible home for the required partner/IBM integration — it stops being a
  checkbox and becomes the product's spine.

## Three agents (no sprawl)

1. **Ingest & Match** — script + pre-indexed clip catalog (JSON + transcripts) → timeline AST.
2. **Compliance** — policy / rights gate + release check. This is where the partner
   integration earns its place.
3. **Assembly** — AST → FCPXML v1.10 + a local FFmpeg low-res web preview.

Deliberately **out of scope:** cloud render farm, Veo or any video generation, Remotion/React
render engine, live 4K ingest during the demo, and any fourth agent.

## What carries over from the current docs unchanged

The design principles already in `architecture.md` are **good and independently arrived at** —
they match what the OpenMontage teardown identified as its strongest ideas. Keep all of them:

- evidence before invention, claims traceable to source
- human approval at high-leverage points, before expensive work
- typed per-stage status (`queued` / `running` / `needs_input` / `failed` / `complete`)
- deterministic reruns: preserve inputs, prompt versions, provider settings
- JSON artifacts so each stage is independently testable and resumable

Those hold under either direction. The fork is about *what the product does*, not how it is built.

## The honest counter-argument

The current README's direction is **better for the stated user**. "Point it at your GitHub page
and get a video, no thinking" is a cleaner story for developers, and FCPXML is useless to
someone without Final Cut. If the target user is a developer with no editor and no footage,
this proposal is the wrong product.

That is the real decision: **who is the user — a developer with no editor, or an editor with
no time?** Everything else follows from it, and it is not a decision to make by merge.
