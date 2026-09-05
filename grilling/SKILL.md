---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user until you reach a shared understanding of the current objective. Map the necessary decisions as a **design tree**. Reuse answers already present in the conversation and defaults explicitly delegated by the user; treat them as settled. Record optional and out-of-scope branches as deferred rather than expanding the interview to cover them.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled — the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round: number each question and give your recommended answer. Then wait for the user's answers before the next round.

Each question should be formatted like so:

```
❓ **Q1** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>
```

Each round the user answers reshapes the tree — settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it — don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report — ask the rest of the frontier now. Put each unresolved decision needed for the current objective to the user and wait; do not reopen settled decisions unless new evidence materially changes their basis.

When all necessary decisions are settled, summarize the agreed design, explicit assumptions, and deferred items for the user's confirmation. The session is complete when the user confirms shared understanding. Preserve that final confirmation before implementing the design, and implement only if the user's request also authorizes it. This gate does not block read-only research, analysis, or the summary itself; silence is not confirmation.
