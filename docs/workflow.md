# Product workflow

## Happy path

> Updated 2026-07-29 for the editorial-assembly direction.

1. The user supplies a **brief or script** and points at source material — a repository, screen
   recordings, screenshots, or a licensed clip catalog.
2. Tools index that material: transcripts, shot boundaries, repo evidence, thumbnails.
3. **Ingest & Match** proposes a timeline: which piece of source material carries which line.
4. *(Optional)* The user reviews the proposed cut. **This step is skippable** — the default run
   is zero-touch and proceeds straight to compliance and assembly.
5. **Compliance** clears every asset for rights and licensing, and flags anything unusable.
6. **Assembly** emits an **FCPXML** for a human editor *and* a **preview MP4** for everyone else.
7. An automated quality gate checks the cut against measurable thresholds before export.
8. The user watches the preview in the browser — or, if they want to, opens the FCPXML in Final
   Cut to finish it. Neither requires the other.

**The default path requires no human input at any point** — but that is not the goal in itself.
The goal is that the tedious work (logging, syncing, clip-hunting, licence checks, caption
timing, audio levelling, export settings) is gone, while every decision a person would *want*
to make stays one gesture away and is never forced on them.

Both exports come from the same TimelineAST, so they never drift apart. Someone with no editor
gets a watchable video; someone with an editor gets a real starting timeline. Neither is a
second-class path.

## Failure and recovery

Every stage should report a typed status: `queued`, `running`, `needs_input`, `failed`, or `complete`. A failed media provider should be retryable without repeating repository analysis or rewriting the approved script.

If the repository does not contain enough visual evidence, the intake stage should pause with a focused request for screenshots rather than inventing product behavior.
