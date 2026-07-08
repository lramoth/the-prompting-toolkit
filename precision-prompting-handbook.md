# The Precision Prompting Handbook

Getting the most out of any model — from the strongest to the cheapest.

The core insight: **the leverage is in the request, not in incantations.** Prompts that instruct a model to "be rigorous" buy a little. Requests that are themselves rigorous — precise deliverable, real context, checkable success condition — buy a lot, and they work on every model because they reduce the amount of guessing the model has to do. A weaker model executing a well-specified request beats a stronger model interpreting a vague one surprisingly often.

Ten practices, roughly in order of payoff.

---

## 1. Name the deliverable and its destination

State what you want *and what you'll do with it*. The downstream use silently answers a dozen questions — length, tone, format, depth — that the model otherwise guesses.

> **Weak:** "Write something about our Q2 results."
> **Strong:** "Write a 5-sentence summary of Q2 results for the board pre-read. They've seen the full deck; this is the reminder paragraph at the top."

If you can't name the downstream use, you haven't finished deciding what you want. Finish deciding first.

## 2. Move judgment upstream, execution downstream

The single biggest lever. Decisions you've already made are free to execute; decisions you delegate are where models go wrong. Spend your expertise (or a stronger model's) writing the *decisions down*, then let any model execute them.

> **Weak:** "Make the reverb sound less metallic."
> **Strong:** "Add two allpass filters, 5–12 ms delay, g≈0.5, only when size ≥ 0.5. Insert after the comb bank."

The weak version asks the model to be a domain expert. The strong version only asks it to be a competent executor — a much easier bar. This is also how to ration access to a stronger model: use it to produce specs, playbooks, and decisions, then hand execution to the cheaper one. Code is cheap; judgment is what you're rationing.

## 3. Show, don't describe

Adjectives ("professional," "concise," "engaging") mean different things to you and the model. Examples don't. One example of the output you want outperforms a paragraph describing it; a good/bad pair outperforms almost anything.

- Rewriting text? Paste a paragraph in the target voice.
- Want a format? Sketch the skeleton: "Output like: `[verdict] — [one-line reason] — [risk]`."
- Reviewing work? Show one finding written the way you want findings written.

Also: paste the actual material. "My function that parses dates" is a description; the function is evidence. Models reason far better over material in front of them than over your summary of it.

## 4. Make success mechanically checkable

Convert "did it reason correctly?" into "did the check pass?" — a question any model handles reliably. Before sending, ask: *how will I know this is right without re-doing the work myself?*

- State acceptance criteria: "Done means: compiles, all tests pass, no new warnings."
- For factual work: "Cite the source line/URL for every figure."
- For analysis: "Show the calculation, not just the result, for any number in the conclusion."
- Where possible, build the check into the environment — a test suite, a validator, an exit code. A check the model must satisfy beats a virtue you asked it to have.

## 5. State constraints with their *why*

A bare rule gets "helpfully" optimized away; a rule with a reason gets respected — and correctly generalized to cases you didn't anticipate.

> **Weak:** "Don't reorder the dropdown options."
> **Strong:** "Don't reorder the dropdown options — the attachment maps index to stored value, so reordering silently corrupts saved data."

Write guardrails at the point of use, not in a distant preamble. The landmine warning belongs next to the landmine.

## 6. Scope small, end verifiable

One well-specified task per request. Big loose requests fail in ways that are expensive to even diagnose; small scoped ones limit the blast radius of any single bad reasoning step.

- Prefer three sequential requests with a checkpoint between them over one mega-request.
- End each unit of work in a verifiable state: tested, summarized, committed, reviewed — whatever "known good" means in your domain.
- If a request has an "and also..." in it, that's usually the seam to cut at.

## 7. Give an escape hatch

Models default to guessing rather than stopping. Make stopping legal:

> "If a decision requires judgment not covered here, ask me instead of guessing."
> "If the premise of this question is wrong, say so — don't answer around it."
> "If you can't verify a claim from the material provided, label it as unverified inline."

Stronger models know when they're out of their depth slightly more often; weaker ones need it as an explicit rule. Either way, an explicit escape hatch is cheap and prevents the worst failure mode: confident fabrication in the gaps of your spec.

## 8. Demand diagnosis before treatment

The classic failure on any nontrivial fix is pattern-matching a plausible-looking change. Split the request:

> "Before proposing a fix, state the root cause and point to the specific line/clause/figure that causes it. Then fix that."

Even better: when you already know the root cause, state it in the request. A model that can't relitigate the diagnosis can't "fix" the symptom by fiddling with the wrong knob. This applies beyond code — to editing arguments, debugging spreadsheets, revising strategy docs.

## 9. Iterate on the request, not just the conversation

When output misses, the instinct is to pile corrections into the chat. Sometimes right — but if you're three corrections deep, the original request was underspecified, and the context is now polluted with failed attempts.

- Fold what you learned back into a fresh, complete request. You'll often get a better result in one shot.
- Push back with *information*, not displeasure: "that's wrong" invites capitulation or stubbornness; "that's wrong because X" invites re-derivation.
- Keep a personal library of requests that worked. A proven prompt is an asset; reuse and refine it.

## 10. Spend model quality where errors are expensive

Match the model (and its thinking budget) to the cost of being wrong, not the difficulty of the task.

- High cost-of-error (irreversible actions, numbers driving decisions, anything sent under your name): strongest model, extended thinking, explicit verification step.
- Low cost-of-error (brainstorms, drafts you'll rewrite, throwaway scripts): cheap and fast is correct — extra rigor is waste.
- "Think hard before editing" isn't a magic phrase; it's permission to spend tokens on the analysis pass. Grant it for the genuinely tricky items and skip it elsewhere.

---

## What doesn't work

**Incantations.** "You are a world-class expert," "take a deep breath," threat and urgency framing. Marginal at best on modern models. The durable technique is the artifact trail — specs, examples, checks — not the magic words.

**Mega-manuals that fire on everything.** A 2,500-word operating manual prepended to every message taxes every request to buy rigor on the few that need it, and models deprioritize rules that are chronically irrelevant. Put the rule where the work is: in the request that needs it.

**Asking one model to impersonate a better one.** Prompts change behavior (structure, checking habits, calibration), not capability. You can get "the same model with better habits" — real and worth having — but not a tier upgrade.

**Rubber-stamp verification.** "Double-check your work" appended to a request mostly produces a sentence claiming the work was checked. Verification has to be *structural*: a separate pass, a test, a required derivation — something that can actually fail.

---

## Before you hit send

1. Could a stranger execute this request without asking me anything? If not, what would they ask? Answer it in the prompt.
2. Is every judgment call either decided by me or explicitly delegated ("your call on X")?
3. Does the model have the actual material, or my description of it?
4. How will I know the output is right without redoing the work?
5. Does it know what to do when it hits the edge of the spec — ask, flag, or stop?

---

## A request template

For nontrivial tasks. Delete what doesn't apply; most requests need only half of it.

```
DELIVERABLE: [what, in what format, roughly what size]
USED FOR: [who reads/runs it and what they'll do with it]
MATERIAL: [paste or attach the actual thing — code, data, draft, source]
DECISIONS ALREADY MADE: [constraints, choices, and the why behind non-obvious ones]
YOUR CALL: [judgment explicitly delegated to the model]
DONE MEANS: [checkable acceptance criteria]
IF STUCK: [ask / flag inline / stop — pick one]
OUT OF SCOPE: [what not to touch, and why it's tempting]
```

The pattern behind all of it: every practice here removes a guess the model would otherwise have to make. Precision in, precision out.
