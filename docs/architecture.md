# Architecture

RepoReel is designed as a sequence of small, inspectable agents connected by explicit artifacts. Each stage should be retryable and should leave behind enough structured output for a human to review or resume a job.

## Pipeline

1. **Repository Intake Agent** accepts a GitHub URL, optional screenshots, and branding preferences.
2. **Repository Analyst** extracts project facts, user-facing features, setup steps, and evidence from the repository.
3. **Story Planner** turns those facts into a single product claim and a short narrative.
4. **Storyboard Agent** maps the narrative to scenes, assets, on-screen text, and timing.
5. **Voice Agent** generates narration aligned to the scene timings.
6. **Music Agent** selects or generates a background track and ducking plan.
7. **Editor Agent** renders the timeline with footage, captions, voice, and music.
8. **Quality Review Agent** checks duration, legibility, audio levels, missing assets, and narrative coherence.

## Core artifacts

```text
ProjectBrief   repository facts and selected product value proposition
Script         narration, captions, and timing targets
Storyboard     ordered scenes with asset requirements
Timeline       render-ready media, transitions, and audio mix
ReviewReport   automated checks plus human approval status
```

The first version should keep these artifacts as JSON so each agent can be tested independently and jobs can resume after failures.

## Design principles

- Evidence before invention: claims should be traceable to repository content or user-provided assets.
- Human approval at high-leverage points: approve the project brief and storyboard before expensive rendering.
- Provider-agnostic media adapters: voice, music, and rendering providers should be replaceable.
- Deterministic reruns: preserve inputs, prompt versions, and provider settings for every job.
