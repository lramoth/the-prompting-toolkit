# The Agentic Coding Handbook

Getting the most out of a coding agent — Claude Code, Cowork, or any model driving a repo.

This is the coding-specific counterpart to the general Precision Prompting Handbook. Everything there still applies; what changes in agentic coding is that the model acts over many steps with your tools, so a bad early decision compounds. The general handbook removes guesses from a single request; this one removes guesses from a *session*. The organizing idea: **build an artifact trail that does the reasoning-shaped work in advance**, so any model — strong or cheap — is executing decisions rather than making them.

Eight practices, roughly in order of payoff.

---

## 1. Freeze decisions into artifacts before the session starts

The biggest lever, and where to spend your strongest model (or your own expertise). An agent session goes well when the judgment was done beforehand and written down:

- **Specs at implementation depth.** Not "add a distortion level control" but the parameter ID, the insertion point, the smoothing pattern. If the spec answers the questions the agent would otherwise guess at, the session becomes execution.
- **Playbooks: symptom → exact change.** For recurring domain judgment, write recipes: "reverb sounds metallic → two allpasses 5–12 ms, g≈0.5, only when size ≥ 0.5." A playbook entry costs you five minutes once and saves a wrong derivation every session after.
- **Approved mockups and frozen interfaces.** UI mockups approved before implementation; param IDs, schemas, and API signatures frozen before code that depends on them. Agents are good at building *to* a fixed target and bad at inventing one mid-session.

Rationing rule: when your access to the stronger model is limited, use it to write specs and decisions, not code. Code is cheap; judgment is what you're rationing.

## 2. Maintain a NOTES.md — the project's external memory

Sessions forget; the repo remembers. Keep one living file (NOTES.md, TODO.md — the name doesn't matter) holding: the backlog with each item written at spec depth, decisions made and their why, open questions parked for you, and the playbook. Two rules make it work:

- **Write backlog items so the root cause is already stated.** "Decay display is wrong because it shows raw seconds instead of normalized slider travel" — an agent can't 'fix' that by fiddling with constants, because the diagnosis is already pinned.
- **End every session by updating it.** The five minutes spent writing "what changed, what's next, what surprised us" is what makes the *next* session start warm instead of cold.

## 3. Write CLAUDE.md as a landmine map, not a style guide

CLAUDE.md is loaded every session — so it must earn its tokens. The highest-value content is **invariants and gotchas with their why**:

> "Don't reorder the segmented control options — SegmentedAttachment maps index==value, so reordering corrupts saved presets."

A model that knows *why* won't helpfully "simplify" the thing away. Every time an agent steps on a landmine, document it in CLAUDE.md before the next session — that's how the file grows: from incidents, not from imagination. Keep it short; move anything long (playbooks, backlog) to NOTES.md and reference it. What belongs: build/test commands and how to read their results, engine invariants and gotchas, session conventions (see #5), and pointers to the artifact trail.

## 4. Make wrongness mechanically detectable

No model can listen to your plugin, see your rendered UI, or feel your app's latency. Convert every "did it reason correctly?" into "did the check pass?":

- A test suite the agent must run, with **real exit-code checks** — agents will happily read a wall of output as success unless told the criterion is the exit code.
- Validators as gates: linters, `auval`/`pluginval`-style external checkers, schema validation, screenshot diffing — whatever your domain has.
- When you ask for a fix, ask for **the failing test first**. A fix that comes with the test that catches the regression is verified; a fix alone is a claim.

Checks the agent must satisfy beat virtues you asked it to have. This is also what makes cheaper models viable: reliability comes from the harness, not the model.

## 5. Scope sessions small; end them verifiable

One well-specified batch per session. Adopt a milestone convention and put it in CLAUDE.md: **every session ends compiling, tested, committed.** This limits the blast radius of any one bad reasoning step — a weaker model doing one well-specified batch beats it doing three loosely-specified ones. Corollaries: commit before letting an agent attempt anything risky (cheap rollback), and if a session goes sideways, prefer reverting and re-prompting fresh over patching a polluted context.

## 6. Demand root-cause-before-fix, every time

The classic agent failure is pattern-matching a plausible fix. Standing rule (belongs in CLAUDE.md too):

> "Before editing, state the root cause and cite the specific line that causes it."

Combined with #2's pre-diagnosed backlog items, this closes the loop: either you pinned the diagnosis in advance, or the agent must pin it before touching code. Never accept a diff whose explanation is "this should fix it."

## 7. Tell it when to stop

Agents default to guessing rather than stopping — and in an agentic session a guess becomes committed code. Standing rule:

> "If a decision requires judgment not covered by the notes, park it in NOTES.md under 'Open questions' and ask the owner instead of guessing."

Stronger models know when they're out of their depth slightly more often; weaker ones need it as an explicit rule. The parking lot matters: "stop and ask" without a place to park stalls the session; with one, the agent finishes everything else in the batch and hands you a tidy list of real decisions.

## 8. Ration thinking, not just model choice

Match reasoning effort to the item, not the session. For genuinely tricky items — concurrency, numerical edge cases, anything touching a frozen interface — say "think hard about this before editing." It's not a magic phrase; it's permission to spend tokens on the analysis pass. For mechanical items in a good spec, skip it: extra deliberation on a well-specified rename is pure waste.

---

## What doesn't work

**Vibes-based sessions.** "Clean up the codebase," "make it more robust" — unbounded scope plus delegated judgment is the worst combination in agentic work. Every failure mode above fires at once.

**Trusting green-looking output.** Agents narrate success fluently. Trust exit codes, diffs, and test results you can read yourself — never the summary sentence.

**Mega-prompts instead of artifacts.** A 2,000-word session prompt is written once and decays immediately. NOTES.md, CLAUDE.md, and the test suite persist, compound, and are versioned with the code. The durable technique is the artifact trail, not the incantation.

**Letting scope creep ride.** Agents love to refactor adjacent code "while they're in there." Standing rule: modify only what the task names; flag the rest in NOTES.md.

---

## Before you start a session

1. Is the batch written at spec depth — could the agent execute it without making a judgment call I'd want to make myself?
2. For each fix item: is the root cause stated, or am I requiring the agent to state it first?
3. What check makes success mechanical (test, validator, exit code) — and does the agent know that's the criterion?
4. Does CLAUDE.md warn about every landmine near this batch, with the why?
5. Does the agent know where to park what it can't decide?
6. Is the repo committed, so the worst case is a revert?
