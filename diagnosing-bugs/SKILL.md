---
name: diagnosing-bugs
description: Diagnosis loop for hard bugs and performance regressions. Use when the user says "diagnose"/"debug this", or reports something broken/throwing/failing/slow.
---

# Diagnosing Bugs

A discipline for hard bugs. Select the task mode before applying the phases; following its scope is not a skipped step.

## Task scope

- **Diagnosis, explanation, or read-only investigation:** use existing code, logs, traces, and non-mutating checks. Deliver the evidence, supported cause or ranked hypotheses, exclusions, remaining uncertainty, and the smallest repair recommendation. Do not enter the fix phase or make diagnosis completion depend on a fixed bug.
- **An explicitly requested fix:** perform the authorized repair and its verification. A diagnosis or skill invocation alone does not authorize code changes, repository-state changes, production instrumentation, or an architecture rewrite.

Choose feedback-loop techniques within that scope. New instrumentation, stress tests, or Git bisection must respect their actual operational effects and applicable authorization requirements. Temporary production instrumentation requires explicit user approval. Continue independent read-only analysis while a needed permission or artifact is unavailable.

When exploring the codebase, read `CONTEXT.md` (if it exists) to get a clear mental model of the relevant modules, and check ADRs in the area you're touching.

## Phase 1 — Build a feedback loop

Prioritize a **tight** pass/fail signal for the bug — one that goes red on _this_ bug. Bisection, hypothesis-testing, and instrumentation can then consume it. When no runnable reproduction is available, use the read-only analysis branch below rather than treating missing reproduction as missing evidence of every kind.

Invest effort in a useful feedback loop within the authorized scope; do not repeat ineffective attempts when the next useful step is read-only analysis or requires unavailable access.

### Ways to construct one — try them in roughly this order

1. **Failing test** at whatever seam reaches the bug — unit, integration, e2e.
2. **Curl / HTTP script** against a running dev server.
3. **CLI invocation** with a fixture input, diffing stdout against a known-good snapshot.
4. **Headless browser script** (Playwright / Puppeteer) — drives the UI, asserts on DOM/console/network.
5. **Replay a captured trace.** Save a real network request / payload / event log to disk; replay it through the code path in isolation.
6. **Throwaway harness.** Spin up a minimal subset of the system (one service, mocked deps) that exercises the bug code path with a single function call.
7. **Property / fuzz loop.** If the bug is "sometimes wrong output", run 1000 random inputs and look for the failure mode.
8. **Bisection harness.** If the bug appeared between two known states (commit, dataset, version), automate "boot at state X, check, repeat" so you can `git bisect run` it.
9. **Differential loop.** Run the same input through old-version vs new-version (or two configs) and diff outputs.
10. **HITL bash script.** Last resort. If a human must click, drive _them_ with `scripts/hitl-loop.template.sh` so the loop is still structured. Captured output feeds back to you.

Build the right feedback loop, and the bug is 90% fixed.

### Tighten the loop

Treat the loop as a product. Once you have _a_ loop, **tighten** it:

- Can I make it faster? (Cache setup, skip unrelated init, narrow the test scope.)
- Can I make the signal sharper? (Assert on the specific symptom, not "didn't crash".)
- Can I make it more deterministic? (Pin time, seed RNG, isolate filesystem, freeze network.)

A 30-second flaky loop is barely better than no loop; a 2-second deterministic one is tight — a debugging superpower.

### Non-deterministic bugs

The goal is not a clean repro but a **higher reproduction rate**. Loop the trigger 100×, parallelise, add stress, narrow timing windows, inject sleeps. A 50%-flake bug is debuggable; 1% is not — keep raising the rate until it's debuggable.

### When you genuinely cannot build a loop

List what you tried and why reproduction is unavailable in the current authorized environment. Continue read-only code tracing, log comparison, and falsifiable hypothesis analysis. Separate confirmed facts from hypotheses and name the evidence that could distinguish them. Ask for a specific missing artifact or access only when it is needed for the next blocked step; temporary production instrumentation still requires explicit user approval. This branch does not waive repair validation or authorize speculative edits. Never describe an untested hypothesis as a verified fix.

### Completion criterion — a tight loop that goes red

For the runnable-reproduction branch, Phase 1 is done when the loop is **tight** and **red-capable**: you can name **one command** — a script path, a test invocation, a curl — that you have **already run at least once** (paste the invocation and its output), and that is:

- [ ] **Red-capable** — it drives the actual bug code path and asserts the **user's exact symptom**, so it can go red on this bug and green once fixed. Not "runs without erroring" — it must be able to _catch this specific bug_.
- [ ] **Deterministic** — same verdict every run (flaky bugs: a pinned, high reproduction rate, per above).
- [ ] **Fast** — seconds, not minutes.
- [ ] **Agent-runnable** — you can run it unattended; a human in the loop only via `scripts/hitl-loop.template.sh`.

Use the runnable command for Phase 2 when available. Otherwise follow the read-only analysis branch above and mark reproduction as unverified; do not claim this reproduction criterion passed.

## Phase 2 — Reproduce + minimise

Run the loop. Watch it go red — the bug appears.

Confirm:

- [ ] The loop produces the failure mode the **user** described — not a different failure that happens to be nearby. Wrong bug = wrong fix.
- [ ] The failure is reproducible across multiple runs (or, for non-deterministic bugs, reproducible at a high enough rate to debug against).
- [ ] You have captured the exact symptom (error message, wrong output, slow timing) so later phases can verify the fix actually addresses it.

### Minimise

Once it's red, shrink the repro to the **smallest scenario that still goes red**. Cut inputs, callers, config, data, and steps **one at a time**, re-running the loop after each cut — keep only what's load-bearing for the failure.

Why bother: a minimal repro shrinks the hypothesis space in Phase 3 (fewer moving parts left to suspect) and becomes the clean regression test in Phase 5.

Done when **every remaining element is load-bearing** — removing any one of them makes the loop go green.

Complete reproduction and minimisation before claiming a reproduced bug. If they are unavailable, continue only the read-only analysis branch with the limitation stated explicitly.

## Phase 3 — Hypothesise

Generate **3–5 ranked hypotheses** before testing any of them. Single-hypothesis generation anchors on the first plausible idea.

Each hypothesis must be **falsifiable**: state the prediction it makes.

> Format: "If <X> is the cause, then <changing Y> will make the bug disappear / <changing Z> will make it worse."

If you cannot state the prediction, the hypothesis is a vibe — discard or sharpen it.

**Show the ranked list to the user before testing.** They often have domain knowledge that re-ranks instantly ("we just deployed a change to #3"), or know hypotheses they've already ruled out. Cheap checkpoint, big time saver. Don't block on it — proceed with your ranking if the user is AFK.

## Phase 4 — Instrument

In read-only diagnosis, inspect existing instrumentation and captured evidence only. Adding instrumentation or changing a running system requires the corresponding authorization from Task scope; presenting hypotheses is not approval to mutate the system.

Each probe must map to a specific prediction from Phase 3. **Change one variable at a time.**

Tool preference:

1. **Debugger / REPL inspection** if the env supports it. One breakpoint beats ten logs.
2. **Targeted logs** at the boundaries that distinguish hypotheses.
3. Never "log everything and grep".

**Tag every debug log** with a unique prefix, e.g. `[DEBUG-a4f2]`. Cleanup at the end becomes a single grep. Untagged logs survive; tagged logs die.

**Perf branch.** For performance regressions, logs are usually wrong. Instead: establish a baseline measurement (timing harness, `performance.now()`, profiler, query plan), then bisect. Measure first, fix second.

## Phase 5 — Fix + regression test

Apply this phase only when the user has requested a fix. For diagnosis-only work, deliver the diagnosis report described in Task scope instead.

Write the regression test **before the fix** — but only if there is a **correct seam** for it.

A correct seam is one where the test exercises the **real bug pattern** as it occurs at the call site. If the only available seam is too shallow (single-caller test when the bug needs multiple callers, unit test that can't replicate the chain that triggered the bug), a regression test there gives false confidence.

**If no correct seam exists, that itself is the finding.** Note it. The codebase architecture is preventing the bug from being locked down. Flag this for the next phase.

If a correct seam exists:

1. Turn the minimised repro into a failing test at that seam.
2. Watch it fail.
3. Apply the fix.
4. Watch it pass.
5. Re-run the Phase 1 feedback loop against the original (un-minimised) scenario.

## Phase 6 — Cleanup + post-mortem

For an authorized repair, required before declaring the fix verified:

- [ ] Original repro no longer reproduces (re-run the Phase 1 loop)
- [ ] Regression test passes (or absence of seam is documented)
- [ ] Temporary `[DEBUG-...]` instrumentation introduced by this task is removed; existing instrumentation is preserved
- [ ] Reproduction materials and user-facing prototypes are listed in the report and retained; no user files or delivered artifacts are automatically deleted or moved
- [ ] The supported cause and verification are stated in the final report; include them in a commit / PR message only when that operation is explicitly authorized

If a required repair check cannot run, report completed work and remaining verification separately; do not claim a verified fix. **Then consider: what would have prevented this bug?** If the answer involves architectural change (no good test seam, tangled callers, hidden coupling), describe it as an optional follow-up recommendation with the evidence. Do not automatically invoke `/improve-codebase-architecture` or make its completion a condition of finishing this task.
