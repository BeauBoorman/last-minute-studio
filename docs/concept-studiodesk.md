# StudioDesk — the concept our research produced

This is the design a week of research and three adversarial review passes converged on. It is
written out in full so it can be argued with on the merits rather than accepted or ignored.

## The pitch

**A secure producer's assembly desk.** A multi-agent *editorial* system: ingest licensed
footage and a script or brief → rights clearance → media indexing → script-matching →
rough-cut assembly → **native FCPXML export**, finished by a human editor in Final Cut.

**It is not an AI-video generator, and that is the entire strategic bet.**

## Why not generate the video

Four reasons, in descending order of how much they matter:

1. **"AI slop" is the default failure of this category.** A fully generated 60-second video is
   the most common submission at an agentic-video hackathon. Under the Google-only runtime
   constraint we cannot out-render a Veo-class demo, so competing on generation means
   competing where we are weakest against the largest field.
2. **Judging rewards a coherent product, not a clip.** *Design* is scored as "a complete,
   coherent product experience beyond proof-of-concept" and *Potential Impact* as "a credible
   solution to real problems for real audiences." A working editorial pipeline is a real
   studio workflow. A generated video is an output.
3. **Rights clearance gives the partner integration a real job.** The required partner/IBM
   integration is otherwise a checkbox bolted to the side. As a compliance and rights gate over
   licensed footage it becomes the product's spine — and it is legible to a judge in one
   sentence.
4. **Cutting rendering takes cloud spend to zero.** Credit amounts are unknown until
   registration. A design whose expensive stage runs locally cannot be killed by a credit cap.

## The three agents

1. **Ingest & Match** — script + pre-indexed clip catalog (JSON + transcripts) → timeline AST.
2. **Compliance** — policy / rights gate + release check. Where the partner integration lives.
3. **Assembly** — AST → FCPXML v1.10 + a local FFmpeg low-res web preview.

Three, not seven, deliberately. Agent count multiplies per-run inference cost against an
unknown credit budget, and *Design* scores coherence, not surface area. Everything else in the
pipeline is a **deterministic tool**, not an agent — tools are testable, cheap, and cannot
hallucinate.

**Cut on purpose:** cloud render farm, Veo, any video generation, Remotion/React render engine,
live 4K ingest during the demo, and any fourth agent.

## What adversarial review actually found

The design went through a readiness review and a supplement. Verdict: **GO-WITH-GAPS** — start
building, do not freeze architecture until the organizer thread returns.

What the red team attacked and the concept survived: the strategic bet, the three-agent scope,
the render cut, and the FCPXML export target.

What it did **not** survive, stated plainly because it matters more than the design:

- *"Every 'resolved' item in the research is asserted, not exercised."* Framework bootstrap is
  proven (`google-adk` installs, ADK API server returns `/health` 200, sessions create).
  Authenticated LLM turns, `gcloud` deploy, IBM Bob telemetry and FCPXML export are **100%
  unexercised**. Do not read the plan as "de-risked."
- **Every hard dependency is at literal zero** — no GCP project, no IBM Bob, no editing seat,
  no footage, no registration.
- **The sleeper kill shot: a judge who cannot log in.** Flagged as unowned. A submission that
  a judge cannot open scores zero regardless of what it does.
- **The arithmetic:** roughly a four-week working window against ~20 workstreams.

The single highest-leverage fix the review identified is a **weekly demo forcing function** —
something that must run end-to-end on a fixed day, every week, from now.

## The one decision this concept does not settle

**Who is the user: a developer with no editor, or an editor with no time?**

StudioDesk assumes the second. FCPXML is worthless to someone without Final Cut, and "point it
at your repo, get a video" is a cleaner pitch to developers.

**Our recommendation is the editorial direction** — because the developer-with-no-editor market
is exactly where the generated-video field will be crowded, and because it is the version that
plays to an actual editor on this team rather than around them.

But it is a genuine fork, it is worth arguing about, and the argument should happen now rather
than in week five.

## The judge path (decided, not deferred)

The contest requires a hosted project URL. A submission a judge cannot open scores zero
regardless of what it does, so this is settled on day one rather than in the final week.

**What opens in a browser:** a hosted page with a pre-loaded example project. It plays the
**preview MP4** inline and shows the timeline the agents produced, with the rights report
beside it.

**What works without Final Cut:** everything a judge needs. Final Cut is an *export target for
practitioners*, never a requirement for evaluation. The preview MP4 and the on-page timeline
carry the whole story; the FCPXML is offered as a download to prove the export is real.

**Credentials a judge needs:** none. A public demo mode with a prepared example runs
end-to-end with no login, no key, and no install. Any account-gated feature is additive and
clearly marked.

**Verification:** this is checked from a cold browser — a clean profile, logged out, no local
state — by someone who did not build it. That test is the gate, not a formality.
