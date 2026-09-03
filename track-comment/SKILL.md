---
name: track-comment
description: Pull review comments from a GitHub or Alibaba Cloud CodeUp URL, compare them with the matching local checkout, and turn verified findings into concrete issues with modification plans. Use when the user wants review feedback tracked against actual local code and discussed before uncertain issues are finalized.
metadata:
  targets: [codex]
---

# Track Comment

Track review feedback from its remote discussion to the actual local code, then produce an evidence-backed issue. Work read-only unless the user separately asks to change code or mutate a remote issue.

## 1. Pull the discussion

Identify GitHub or CodeUp from the URL. In OMP, use Kimi WebBridge first so the review is read from the user's authenticated browser; fall back to OMP's native Browser only when WebBridge remains unavailable after its normal startup retry or lacks a capability required by the page. In other hosts, prefer an available authenticated connector, API, or CLI, then use the signed-in browser when needed.

Read the review or issue body, patchset or commit identity, changed-file context, inline threads, general comments, replies, resolution state, linked issues, and pagination or collapsed discussions. Record each relevant comment's direct link, file and line, version, status, and replies.

This step is complete when every visible unresolved thread is accounted for, or each inaccessible portion is named explicitly.

## 2. Match the local code

Locate the checkout whose remote repository and branch or commit match the URL. Inspect its Git branch, HEAD, remotes, status, and relevant code before drawing conclusions. Keep these layers distinct:

- Remote review patchset or commit.
- Local HEAD.
- Staged changes.
- Unstaged changes and untracked files when relevant.

Preserve the dirty worktree. Do not fetch, checkout, edit, stage, commit, or push as part of comment tracking. When more than one checkout is plausible or none can be matched reliably, discuss the candidate paths with the user before continuing.

This step is complete when the exact checkout and every code layer used by the analysis are identified, or the missing local context is disclosed.

## 3. Verify and consolidate

Trace the relevant callers and behavior far enough to identify the root problem. Treat comments as claims to verify, not as issues to copy.

- Merge comments with the same root cause; preserve distinct behavior failures.
- Classify each thread as `未处理`, `本地已处理未推送`, `已提交`, `不成立或已过时`, or `待讨论`.
- Separate confirmed defects from design preferences and questions.
- Exclude resolved, approving, withdrawn, duplicate, or non-actionable comments, while briefly accounting for them.
- Derive the smallest viable modification plan from the local code. Name the affected files or symbols, behavior change, compatibility impact, and minimum regression check. Do not implement it unless asked.

## 4. Discuss real ambiguity

Continue directly when code and requirements determine the answer. Ask the user before finalizing an issue when any of these would change the conclusion or solution:

- The comment contradicts the inspected code or targets an older patchset.
- The required business behavior cannot be derived from code, documentation, or discussion.
- Multiple viable solutions have different external behavior, compatibility, data, or operational effects.
- The matching local checkout or code layer is uncertain.

Show the concrete evidence, the decision that is missing, and the smallest set of meaningful options. After the user answers, revise only the affected issue and remove resolved questions.

## 5. Produce the issue

Lead with coverage: platform URL, remote state and version, local checkout and Git layers inspected, and any access gap. For each actionable root problem, provide:

1. Issue title.
2. Conclusion and classification.
3. Comment evidence with direct links.
4. Local-code evidence with file and line or symbol.
5. Problem point, root cause, and actual impact.
6. Minimal modification plan.
7. Acceptance criteria and the smallest meaningful regression check.
8. Open decision, only when discussion is still required.

When no actionable issue remains, say so and account for every excluded comment. Creating, editing, closing, or commenting on a remote issue requires an explicit user request; otherwise the deliverable is the issue text in chat.
