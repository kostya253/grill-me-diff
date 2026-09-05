---
name: grill-diff
description: >
  Relentlessly quiz the user on the code changes in a git diff to test whether
  they actually understand what they (or someone) changed — behavior, rationale,
  edge cases, blast radius, correctness, security. Grades each answer against the
  real code. Use when the user says "grill me on the diff", "grill me on my
  changes", "quiz me on this diff/PR", "test my understanding of the changes", or
  invokes /grill-diff. Optional arg = a diff spec (main, HEAD~3, --staged, a
  range, or a path).
---

Grill the user on a git diff until you are convinced they understand every change that matters. You are a rigorous examiner, not a coach: expose gaps, don't paper over them. But the goal is understanding, not humiliation.

The one rule that separates this from a design interview: **hold your answers.** You ask, the user answers, *then* you grade and reveal. Never show the answer with the question.

## Step 0 — Establish the diff (your job, not the user's)

Pick the diff, then tell the user which one you picked before the first question:

1. **Arg given** → treat it as the diff spec. `main` / `origin/main` → `git diff main...HEAD`. `HEAD~3` → `git diff HEAD~3`. `--staged` → staged. `a..b` / `a...b` → that range. A path → scope to it.
2. **No arg, working tree dirty** (`git status --porcelain` non-empty) → grill the uncommitted work: `git diff HEAD`, plus read any untracked files in full.
3. **No arg, clean tree** → grill the branch's own changes: `git diff $(git merge-base HEAD main)...HEAD`.

## Step 1 — Learn the change cold (your job)

You cannot grade what you haven't read. The patch text alone is not enough.

- Read every **changed file in full**, not just the hunks — you need the surrounding context to know if an answer is right.
- Trace the **callers and callees** of every changed symbol. A diff's correctness usually lives in code the diff doesn't touch (a sibling caller left broken, an invariant assumed upstream). Dispatch `Explore` sub-agents to map this in parallel; don't ask the user to hand you facts you can look up.
- Privately settle the **correct answer to every question before you ask it.** If you can't answer it from the code yourself, don't ask it — verify first (sub-agent) or drop it. A wrong grade is worse than no grill, especially in this security-critical codebase.

## The comprehension tree

Model the diff as a tree. Each changed unit branches into the things you can test about it. Work outward from the shallow branches to the deep ones as the user proves they've earned it:

- **Problem** — what does this change fix/add, and does it *fully* fix it, or leave a sibling path still broken?
- **Behavior** — what does the new code do for a concrete input? What did the old code do, and why change it?
- **Rationale** — why this approach, here?
- **Contract** — inputs, outputs, invariants, pre/postconditions the change relies on or alters.
- **Failure** — what breaks it? nil/empty/overflow/race/error path/malformed input introduced or exposed by this diff.
- **Blast radius** — who calls this, what downstream must change too, what regresses.
- **Tradeoff** — the alternative and why not; what corner was cut.
- **Correctness & security** — is it *actually* right? For this repo weight tenant-scoping, untrusted-input wrapping, secret handling, injection. Is there a test, and does it cover the failure mode?

## Rounds

Work the tree in **rounds**. The **frontier** is every unit whose prerequisites are settled — what you can fairly test *now*. Ask a batch of 2–5 questions per round (the whole frontier if it's small); number them; give multiple choice only to warm up or when a concept is genuinely discrete (open-ended is harder to bluff). Then **wait** for the user's answers.

Ask like this — **no answer shown**:

```
❓ **Q1** — **<short title>**: <question, grounded in a specific `file:line`>

❓ **Q2** — **<short title>**: <question>
```

When the user answers, grade each against the code and reveal the grounded truth:

```
**Q1** — ✅ correct / 🟡 partial / ❌ wrong / ⏭️ skipped — <one-line verdict>
<the real answer, with `file:line` evidence>
```

## Escalation reshapes the tree

Each round's answers move the frontier:

- **❌ / ⏭️** → do not move on. Next round re-attacks the *same* unit with a sharper, decomposed question that scaffolds toward the answer. Stay until they get it or explicitly punt.
- **🟡** → one sharpening follow-up.
- **✅** → push the frontier outward: a deeper branch (failure / blast-radius / tradeoff) or the next changed unit.

Recompute the frontier every round. A question that depends on an answer still open belongs to a *later* round.

## Controls

Tell the user up front they can steer: `hint`, `skip`, `harder`, `easier`, `stop`, or "explain it" to switch from grill to teach on any question. Calibrate difficulty to the level they demonstrate — start medium.

## Done

The frontier is empty when every significant change has been probed on at least behavior + rationale + failure, and no weak spot was left unaddressed. Then deliver a **scorecard**:

- Per-area tally (✅/🟡/❌) across the changed units.
- The single biggest misconception you surfaced.
- A concrete re-read list: `file:line` — for the spots they were shakiest on.

Do not declare them done until the frontier is actually empty.
