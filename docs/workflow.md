# Product workflow

## Happy path

> Updated 2026-07-29 for the editorial-assembly direction.

1. The user supplies a **brief or script** and points at source material — a repository, screen
   recordings, screenshots, or a licensed clip catalog.
2. Tools index that material: transcripts, shot boundaries, repo evidence, thumbnails.
3. **Ingest & Match** proposes a timeline: which piece of source material carries which line.
4. The user reviews the proposed cut — *this is the cheap approval point, before any render*.
5. **Compliance** clears every asset for rights and licensing, and flags anything unusable.
6. **Assembly** emits an **FCPXML** for a human editor *and* a **preview MP4** for everyone else.
7. The user watches the preview in the browser, or opens the FCPXML in Final Cut to finish it.

Both exports come from the same TimelineAST, so they never drift apart. Someone with no editor
gets a watchable video; someone with an editor gets a real starting timeline. Neither is a
second-class path.

## Failure and recovery

Every stage should report a typed status: `queued`, `running`, `needs_input`, `failed`, or `complete`. A failed media provider should be retryable without repeating repository analysis or rewriting the approved script.

If the repository does not contain enough visual evidence, the intake stage should pause with a focused request for screenshots rather than inventing product behavior.
