---
name: research
description: Research technical or factual questions using high-trust primary sources such as official documentation, specifications, source code, papers, and first-party APIs. Use when the user asks for research, source-backed facts, API or documentation investigation, or evidence synthesis.
---

# Research

Investigate the user's question against primary sources. Prefer official documentation, specifications, source code, research papers, and first-party APIs. Trace important claims to the source that owns them.

Use a background agent when supported, authorized, and useful. Otherwise perform the research in the current session.

Return findings directly in the conversation by default. Write a Markdown artifact only when the user explicitly asks to save the research. If the user does not provide a destination, use the operating system's temporary directory rather than a business repository.

For important conclusions:

1. Cite the supporting source near the claim.
2. Separate verified facts from inference.
3. Note conflicts, uncertainty, and missing evidence.
4. Prefer current authoritative sources when information may have changed.

Do not modify business code or repository state as part of research unless the user explicitly expands the task.
