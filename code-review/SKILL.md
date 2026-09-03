---
name: code-review
description: Review uncommitted workspace changes or changes since a branch, tag, commit, or merge-base along separate Standards and Spec axes. Use when the user asks to review a working tree, staged changes, branch, PR, or implementation against requirements.
---

# Code Review

Review changes without modifying files or Git state. Keep the Standards and Spec findings separate.

## Select the comparison mode

### Workspace mode

When the user does not provide a fixed point, inspect both unstaged and staged changes:

```bash
git status --short
git diff
git diff --cached
```

Include untracked files when they are relevant and readable, but do not stage them.

### Fixed-point mode

When the user provides a branch, tag, or commit, first verify that it resolves, then inspect:

```bash
git diff <fixed-point>...HEAD
git log <fixed-point>..HEAD --oneline
```

Stop and report the issue if the fixed point is invalid.

## Standards axis

Read applicable `AGENTS.md`, `CONTRIBUTING.md`, coding standards, and nearby tests. Report only actionable findings supported by the diff and repository evidence. Include file and line locations where possible.

## Spec axis

Use a user-provided specification, ticket, issue, PRD, or explicit conversation requirements. Check whether the change implements the requested behavior without unrelated scope expansion. If no specification exists, skip this axis and state that only Standards review was performed.

Do not require an issue tracker.

## Execution

Run the two axes in parallel when subagents are supported and authorized. Otherwise run them sequentially in the current session. Do not reduce them to one blended conclusion.

Do not execute Git commands that modify repository state. Do not edit code unless the user separately asks for fixes.

## Output

Report confirmed findings first, ordered by severity. Then provide:

1. Standards result.
2. Spec result, or `No spec available`.
3. Verification gaps and residual risks.
