---
name: thermos
description: Run a combined diff-scoped review for correctness, security, breaking changes, developer experience, implementation quality, maintainability, and structural complexity. Use only when the user explicitly invokes $thermos for a branch or PR audit.
---

# Thermos

Run two independent review passes over the same fixed diff, then synthesize one evidence-backed verdict. Keep the workflow read-only.

## Workflow

1. Resolve the exact PR, branch, base revision, or changed-file scope from the invocation. Ask only if the scope cannot be discovered safely.
2. Gather the diff and the minimum full-file context needed to trace changed behavior. Do not report unrelated pre-existing issues.
3. Read both rubrics completely:
   - [`references/correctness-security.md`](references/correctness-security.md)
   - [`references/implementation-quality.md`](references/implementation-quality.md)
4. When subagent delegation is available, launch two independent subagents in parallel. Give both the same fixed diff and context: one applies the correctness/security rubric; the other applies the implementation-quality rubric. If delegation is unavailable, run the two passes sequentially without mixing their intermediate conclusions.
5. Verify every candidate finding against repository evidence. For medium-or-higher findings on a PR, inspect existing PR discussion only after the independent passes, then validate and deduplicate it.
6. Synthesize findings first, highest severity first. Merge duplicates, explain disagreements, include concrete `file:line` evidence and failure impact, then give a brief overall verdict.

Do not modify code, stage files, commit, push, post review comments, or prepare a merge-ready PR unless the user separately requests that action.
