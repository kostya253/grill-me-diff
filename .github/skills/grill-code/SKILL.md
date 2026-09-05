---
name: grill-code
description: >
  Relentlessly quiz the user on a piece of code — a file, directory, or symbol —
  to test whether they actually understand it: behavior, rationale, contracts,
  edge cases, blast radius, correctness, security. Grades each answer against the
  real code. Use when the user says "grill me on this code/file", "quiz me on
  <thing>", "test my understanding of <module>", or invokes /grill-code. Optional
  arg = the target (a path, glob, or symbol name).
---

Grill the user on a chosen piece of code until you are convinced they understand it. You are a rigorous examiner, not a coach: expose gaps, don't paper over them. But the goal is understanding, not humiliation.

The one rule that separates this from a design interview: **hold your answers.** You ask, the user answers, *then* you grade and reveal. Never show the answer with the question.

## Step 0 — Establish the target (your job, not the user's)

- **Arg given** → it's the scope: a file path, a directory/glob, or a symbol name (locate its definition + call sites).
- **No arg** → you cannot grill "code in general." Ask the user what to grill on — a file, a module, or a symbol — offering to infer it from what they're currently working on or recently edited. Get a concrete scope before the first question.

Tell the user the scope you settled on before you start.

## Step 1 — Learn the target cold (your job)

You cannot grade what you haven't read.

- Read the target **in full**. For a symbol, read its definition *and* its callers/callees — understanding is relational.
- Trace the flow end to end: what calls in, what it calls out, what state it touches. Dispatch `Explore` sub-agents to map this in parallel; don't ask the user for facts you can look up yourself.
- Privately settle the **correct answer to every question before you ask it.** If you can't answer it from the code yourself, don't ask it — verify first (sub-agent) or drop it. A wrong grade is worse than no grill, especially in this security-critical codebase.

## The comprehension tree

Model the target as a tree. Each unit branches into the things you can test about it. Work outward from the shallow branches to the deep ones as the user proves they've earned it:

- **Behavior** — what does it do for a concrete input? Walk a real value through it.
- **Rationale** — why is it written this way, and why here?
- **Contract** — inputs, outputs, invariants, pre/postconditions; what the caller must guarantee.
- **Failure** — what breaks it? nil/empty/overflow/race/error path/malformed input. What happens on each?
- **Blast radius** — who depends on this, what breaks if you change it, what the ripple is.
- **Tradeoff** — the alternative design and why this one; what corner it cuts.
- **Correctness & security** — is it *actually* right? For this repo weight tenant-scoping, untrusted-input wrapping, secret handling, injection. Is it tested, and does the test cover the failure mode?

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
- **✅** → push the frontier outward: a deeper branch (failure / blast-radius / tradeoff) or the next unit.

Recompute the frontier every round. A question that depends on an answer still open belongs to a *later* round.

## Controls

Tell the user up front they can steer: `hint`, `skip`, `harder`, `easier`, `stop`, or "explain it" to switch from grill to teach on any question. Calibrate difficulty to the level they demonstrate — start medium.

## Done

The frontier is empty when every significant unit of the target has been probed on at least behavior + rationale + failure, and no weak spot was left unaddressed. Then deliver a **scorecard**:

- Per-area tally (✅/🟡/❌) across the units.
- The single biggest misconception you surfaced.
- A concrete re-read list: `file:line` — for the spots they were shakiest on.

Do not declare them done until the frontier is actually empty.
