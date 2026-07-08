# CLAUDE.md

<!-- Template: fill the bracketed sections for your repo, delete these comments,
     save as CLAUDE.md in the repo root. Keep it short — this file is loaded
     every session and must earn its tokens. Grow the Invariants section from
     real incidents: every time a session steps on a landmine, document it here. -->

## Project

[One paragraph: what this is, the tech stack, where the entry points are.]

## Build & test

- Build: `[command]`
- Test: `[command]`
- Validate: `[external checker, linter, etc.]`
- **Success criterion is the exit code, not the output text.** Check exit codes for real; a wall of log output is not a pass.

## Session conventions

- Every session ends compiling, tested, committed. No exceptions — if the batch can't get there, shrink the batch.
- Before editing, state the root cause and cite the specific line that causes it. No fix without a diagnosis.
- For any bug fix, write the failing test first, then the fix.
- Modify only what the task names. If you spot problems outside scope, note them in NOTES.md — don't fix them unasked.
- If a decision requires judgment not covered by the task or the notes, park it in NOTES.md under "Open questions" and ask the owner instead of guessing. Then continue with the rest of the batch.

## Where things live

- `NOTES.md` — backlog (items written with root cause stated), decisions and their why, playbook, open questions. Read it at session start; update it at session end.
- [Specs / mockups / schema files and their locations.]
- [Test suite location and how it's organized.]

## Invariants & gotchas

<!-- The landmine map. Every entry: the rule AND the why — a model that knows
     why won't "helpfully" simplify the thing away. Examples of the pattern: -->

- Don't reorder [the segmented control options] — [SegmentedAttachment maps index==value, so reordering silently corrupts saved presets].
- Never change [parameter IDs / schema field names] — [they're frozen; persisted data depends on them].
- [Component X] must [invariant] because [consequence of violating it].

## Interfaces that are frozen

[Param IDs, API signatures, file formats, DB schemas that must not change without owner approval. List them explicitly so "improving" them is recognizably out of bounds.]
