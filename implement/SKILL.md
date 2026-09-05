---
name: implement
description: Implement a specified coding task using test-first development where executable behavior changes, verify the result, review the final diff, and leave all changes uncommitted by default.
---

# Implement

Implement the task specified by the user, specification, or ticket.

## Scope gate

Before modifying production code:

1. Read the relevant implementation and existing tests.
2. Identify the externally observable behavior being changed.
3. Identify affected callers, interfaces, configuration, resources, and concurrency contexts.
4. State material assumptions.
5. Keep the change limited to the requested behavior.

Do not introduce speculative abstractions, unrelated refactoring, unused
extension points, or unrequested functionality.

## Mandatory TDD gate

For executable behavior changes, bug fixes, parsing logic, algorithms,
state transitions, public interfaces, and reusable business logic:

1. Identify the highest practical public test seam.
2. Write one failing test.
3. Run the test and confirm that it fails for the expected reason.
4. Implement only enough production code to pass that test.
5. Run the test and confirm that it passes.
6. Repeat one vertical slice at a time.

Do not write all tests first and all implementation afterward.

TDD may be skipped only for:

- documentation-only changes;
- comment or formatting-only changes;
- generated files;
- configuration values with no executable behavior;
- hardware-dependent behavior that cannot be simulated locally;
- disposable prototypes.

When skipping TDD:

1. State the concrete reason before editing production code.
2. Explain why no practical test seam exists.
3. Perform the smallest available alternative verification.

Task size, confidence, and time pressure are not valid reasons to skip TDD.

## Verification

During implementation:

- run the directly related test after every slice;
- run compilation or type checking regularly;
- do not suppress, weaken, or delete tests to make them pass;
- distinguish existing failures from failures introduced by the change.

At completion:

1. Run all directly related tests.
2. Run the affected module test suite.
3. Run broader tests when practical.
4. Review the complete working-tree diff.
5. Apply the code-review discipline.
6. Report exact commands, exit codes, and verification results.

## Git policy

Read-only Git commands are allowed:

- `git status`
- `git diff`
- `git log`
- `git show`

Leave changes unstaged, uncommitted, and unpushed by default. Execute a Git
state change only when the current user request explicitly authorizes that
operation, and only within the authorized repository and file scope. An
explicit request to commit includes staging only the intended changes needed
for that commit, without asking separately for that necessary step. Preserve
unrelated staged and unstaged changes. This exception does not authorize
automatic staging after ordinary edits, push, branch switching, reset,
history rewriting, or unrelated Git operations.

## Completion report

Report:

1. Implementation summary.
2. Files changed.
3. Tests and checks executed.
4. Exact verification results.
5. Remaining risks.
6. Suggested commit message when no commit is authorized; otherwise report the actual outcome of the explicitly authorized Git operation, including anything not completed.
