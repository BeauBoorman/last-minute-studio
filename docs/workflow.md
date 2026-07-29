# Product workflow

## Happy path

1. The user pastes a public GitHub URL.
2. RepoReel scans the repository and presents a short project brief.
3. The user confirms the strongest story and adds screenshots or a logo if needed.
4. RepoReel generates a 60–90 second script and storyboard.
5. The user approves or edits the storyboard.
6. Agents generate narration, music, and any missing visual treatments.
7. The editor assembles a preview.
8. Quality review flags issues such as unreadable captions or unsupported claims.
9. The user exports the final MP4.

## Failure and recovery

Every stage should report a typed status: `queued`, `running`, `needs_input`, `failed`, or `complete`. A failed media provider should be retryable without repeating repository analysis or rewriting the approved script.

If the repository does not contain enough visual evidence, the intake stage should pause with a focused request for screenshots rather than inventing product behavior.
