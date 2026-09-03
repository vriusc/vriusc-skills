---
name: review-me
description: Use only when the user explicitly invokes "/review-me" in Claude Code or "$review-me" in Codex to review a plan, design, or consequential decision one question at a time. Runs a read-only decision-tree interview and ends with a confirmed record in which every consequential branch is resolved, ruled out, or deliberately deferred. For defects in a Git diff use code-review instead.
---

# Review Me

Run an evidence-first decision-tree review. The outcome is a confirmed,
leaf-complete decision record: every consequential branch is resolved, ruled
out with a reason, or deliberately deferred with a safe default.

## Session contract

- Keep exactly one decision question active. Wait for its answer before asking
  the next question.
- Inspect the conversation, repository, files, tools, and other available
  read-only evidence for facts. Put choices to the user; bring facts to them.
- Give a specific recommended answer and its decisive tradeoff with every
  question. Put the recommendation first in a numbered list of two or three
  mutually exclusive options when the decision fits choices.
- Keep the session read-only until the user confirms the closure record. After
  confirmation, act only when the surrounding request already authorized that
  action; otherwise finish with the record.
- Match the user's language.

## 1. Establish the root and the decision frontier

Before asking the first substantive question, read
[`references/review-lenses.md`](references/review-lenses.md) completely.

Recover the review target, desired outcome, constraints, accepted decisions,
and evidence from the invocation and current conversation. Inspect the scoped
environment to replace discoverable unknowns with facts. If no review target
exists, ask only for the target and wait.

Build an internal decision tree. Give each node a stable path such as `1`,
`1.2`, and `1.2.1`, record its dependencies, and track one of these states:
`open`, `answered`, `expanded`, `closed`, or `reopened`. Instantiate every
applicable review lens as a node or an explicit audit item. Order the frontier
by dependency first, then by blast radius: settle the choice that can invalidate
the most descendants before its consequences.

This step is complete when the root is concrete, known facts are evidenced,
every currently visible decision is on the frontier, and the next question is
the highest-leverage unresolved node.

## 2. Traverse one node at a time

Ask one question in this shape:

```text
Decision <path> — <short label>
Why this matters: <one concrete consequence>
Recommendation: <specific choice> — <decisive tradeoff>

1. <recommended choice> (Recommended) — <impact>
2. <alternative> — <impact>
3. <alternative, only when genuinely distinct> — <impact>

<one question>
```

Use a single free-form prompt when fixed choices would falsely narrow the
decision. Explanatory context and option tradeoffs are not extra questions.

After each answer:

1. Record the exact decision and rationale in the node.
2. Resolve ambiguity on the same node before moving on.
3. Ask internally, “Because this is now true, what else must be decided?” Add
   every consequential child to the frontier.
4. Reopen descendants whose assumptions changed.
5. Select the next dependency-ready node with the largest blast radius.

An answer containing a fuzzy quality such as “simple”, “fast”, “secure”,
“later”, or “as needed” creates a measurable child decision. A new actor,
state, integration, time boundary, exception, or fallback creates another child.

This step is complete for a turn only when the answer is either a precise
decision or the next message remains on that same node, and all newly implied
children have been added before another branch is selected.

## 3. Close leaves, not topics

A node becomes a **closed leaf** only when all five closure tests pass:

1. **Choice** — the selected behavior is exact enough to implement or follow.
2. **Boundary** — actors, triggers, preconditions, in-scope behavior, and
   out-of-scope behavior are explicit.
3. **Variants** — the happy path, empty or boundary case, failure, and recovery
   have deterministic outcomes where applicable.
4. **Consequences** — every applicable lens affected by this choice has either
   a resolved child or a recorded reason it does not apply.
5. **Proof** — at least one observable acceptance example distinguishes success
   from failure.

Stress-test a candidate leaf with one concrete boundary example and one hostile
or failure scenario. Any answer that changes expected behavior creates a child
node instead of closing the leaf.

A deliberate deferral closes a leaf only when it records the owner, decision
trigger, safe interim default, and consequence of waiting. If the user chooses
to stop early, summarize the unresolved frontier and label the review
incomplete.

This step is complete only when every descendant of the node is closed and its
expected result can be checked without interpreting placeholders or adjectives.

## 4. Audit and confirm the whole tree

When the frontier appears empty, account for every item in the loaded review
lenses as one of:

- resolved, with the decision-node paths that cover it;
- not applicable, with a concrete reason; or
- deliberately deferred, with owner, trigger, interim default, and consequence.

Resume the one-question loop for any gap. When no gap remains, present a compact
closure record:

```markdown
## Review closure

### Objective and constraints
### Resolved decision tree
### Leaf contracts and acceptance examples
### Evidence and assumptions
### Deliberate deferrals
```

Then ask one final confirmation question: whether the record accurately captures
the user's decisions. A correction reopens the affected node and its descendants.

The review is complete only when every applicable lens is accounted for, no
node is open or dependency-blocked, every leaf passes all five closure tests,
and the user confirms the closure record.
