---
name: prototype
description: Build a disposable prototype to answer a design question about logic, a state model, integration feasibility, or UI direction. Use when the user wants to test an uncertain idea with a minimal runnable experiment before changing production code.
---

# Prototype

Build the smallest disposable experiment that answers the user's design question.

## Select the prototype shape

- For logic or state-model questions, read [LOGIC.md](LOGIC.md).
- For UI direction questions, read [UI.md](UI.md).

State the question and success signal before building. Keep the prototype visibly separate from production code, runnable with one command, and free of persistence unless persistence is itself under test. Surface the relevant state and results so the user can evaluate the experiment.

Do not add production abstractions, production-grade error handling, or unrelated polish. Do not copy prototype code directly into production code; transfer only validated decisions through a separately reviewed implementation.

Do not create or switch branches, write Git state, commit, push, or record a commit as a completion requirement. Leave prototype files uncommitted, do not delete them automatically, and let the user decide their disposition.

## Completion report

Report:

1. The original question.
2. The experiment and observed result.
3. The resulting design conclusion.
4. Limits and assumptions.
5. The uncommitted files created or changed.
