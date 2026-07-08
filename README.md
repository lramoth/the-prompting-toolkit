# The Prompting Toolkit — README

Four documents, one system. Two are **for you** (handbooks you read and apply when writing requests), and two are **for the model** (instructions you paste where the model will load them). Each pair covers one context: everyday chat work, and agentic coding on a repo.

|  | For the human | For the model |
|---|---|---|
| **Chat / general work** | `precision-prompting-handbook.md` | `project-instructions.md` |
| **Agentic coding on a repo** | `agentic-coding-handbook.md` | `CLAUDE-template.md` |

The shared idea behind all four: **model output quality is mostly determined by how many guesses the model has to make.** The human-side documents teach you to remove guesses from your requests; the model-side documents catch the guesses that remain. Neither half is enough alone — pasted instructions can't rescue a vague request, and a precise request still benefits from a model that verifies premises and flags errors.

---

## 1. precision-prompting-handbook.md

**What it is:** Ten practices for writing precise requests, ordered by payoff — from naming the deliverable and its downstream use, through moving judgment upstream, to matching model quality to cost-of-error. Ends with a 5-question pre-send check and a fill-in request template.

**Who it's for:** You (or anyone who prompts). Never pasted into a model.

**Where to put it:** Anywhere you'll actually reopen it — a notes app, a `docs/` folder, printed. The pre-send check and the request template are the parts worth keeping within reach.

**When to use it:** Before writing any nontrivial request, in any tool and with any model — Claude chat, Claude Code, Cowork, other vendors. It's model-agnostic because it works on the request, not the model.

**How to get the most out of it:** Don't try to apply all ten practices at once. Start with the two highest-payoff habits — always name the downstream use (#1), and paste the actual material instead of describing it (#3). Use the full request template only for tasks big enough to justify it; most requests need half of it. When an output disappoints, diagnose which practice was skipped rather than piling corrections into the chat — then rewrite the request (practice #9).

## 2. project-instructions.md

**What it is:** Model-facing working rules (~400 words, deliberately short): verify premises before building on them, diagnose before fixing, re-derive numbers passing through, label guesses inline, answer first, re-derive rather than capitulate under pushback. Each rule is conditional, so casual conversation stays untouched.

**Who it's for:** The model, in chat-based work.

**Where to put it:** claude.ai → Projects → create or open a Project → **Project instructions** → paste. It then loads automatically in every conversation inside that Project. (Works the same anywhere custom/system instructions are supported.)

**When to use it:** For Projects where correctness matters — documents with numbers, analysis, editing work, decisions. Skip it for purely creative or conversational Projects; it buys nothing there.

**How to get the most out of it:** Treat it as a starting point, not scripture — delete rules that don't fit the Project's work, and add project-specific ones (key facts, preferred formats, standing context). Keep it short as you do: instructions load on every message, and models deprioritize rules that are chronically irrelevant. If you find yourself correcting the model the same way twice, that correction probably belongs here as a new rule.

## 3. agentic-coding-handbook.md

**What it is:** Eight practices for running coding-agent sessions, built around one idea: an artifact trail (specs, NOTES.md, playbooks, tests, CLAUDE.md) that does the reasoning-shaped work in advance, so any model executes decisions instead of making them. Covers freezing decisions before sessions, external memory, mechanical verification, small verifiable sessions, root-cause-before-fix, and escape hatches. Ends with a 6-question pre-session check.

**Who it's for:** You, when directing an agent (Claude Code, Cowork with a connected repo, or similar) on real code.

**Where to put it:** Same as the other handbook — your reference shelf. Some people keep a copy in the repo's `docs/` for teammates; it's still human-facing either way.

**When to use it:** When planning a batch of agent work — especially before spending limited stronger-model access (use it on specs, not code), and whenever a session went sideways (the "What doesn't work" section is a post-mortem checklist).

**How to get the most out of it:** The pre-session check is the operational core — run it before starting any batch. The single highest-leverage habit is #1/#2 combined: write backlog items at spec depth with the root cause already stated. If you adopt nothing else, adopt the milestone convention (#5): every session ends compiling, tested, committed.

## 4. CLAUDE-template.md

**What it is:** A fill-in-the-brackets template for a repo's `CLAUDE.md`: build/test commands with the exit-code rule, session conventions (root-cause-before-fix, failing-test-first, park-and-ask), pointers to NOTES.md and specs, and — the highest-value section — invariants and gotchas documented *with their why*.

**Who it's for:** The model, in repo-based agent work.

**Where to put it:** Fill in the bracketed sections, delete the template comments, save as `CLAUDE.md` in the **repo root**, and commit it. Claude Code loads it automatically every session; Cowork does too when that folder is connected. Subdirectory `CLAUDE.md` files work for area-specific rules in large repos.

**When to use it:** Set it up once per repo before the first agent session. Then maintain it continuously — it's a living file, not a one-time config.

**How to get the most out of it:** Grow the Invariants section from *incidents*, not imagination: every time an agent steps on a landmine, document it there before the next session, always with the why (a model that knows why won't "helpfully" simplify the thing away). Keep the file short — it loads every session and must earn its tokens; anything long (backlog, playbooks) goes in NOTES.md with a pointer. Review it occasionally and delete rules that no longer apply — stale rules erode trust in the live ones.

---

## How the pieces work together

A typical flow, chat side: you write the request using the precision handbook's practices; the Project instructions catch what slips through (an unverified premise, an unflagged guess). Coding side: you prepare the batch using the agentic handbook (spec-depth backlog, tests in place, repo committed); CLAUDE.md enforces the session conventions and warns about the landmines; NOTES.md carries memory between sessions.

Three principles that span everything here:

1. **Judgment upstream, execution downstream.** Decide, write the decision down, then delegate execution. This is what makes cheaper models viable and stronger models efficient.
2. **Rigor lives in structure, not incantations.** Checkable success criteria, tests, exit codes, and frozen interfaces outperform "be careful" in any phrasing. Pasted instructions are the smallest part of the system; the artifact trail is the durable part.
3. **Keep model-facing text short and conditional.** Both paste-able files are deliberately small. A 2,500-word manual prepended to everything taxes every message and gets deprioritized; a short rule at the point of use gets followed.

Adapt freely — these are starting points calibrated to general use. The versions that will serve you best are the ones edited down to your actual projects, with your own landmines documented and your own recurring corrections promoted into rules.
