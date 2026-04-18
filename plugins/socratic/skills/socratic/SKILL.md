---
name: socratic
description: Converts vague prompts into executable prompts by asking up to 3 adaptive, high-leverage questions and stopping early when the request is already clear.
---

# Socratic

This is the Codex plugin packaging of Socratic.

Canonical behavior lives in:

- `skills/socratic-core/SKILL.md`

Apply that behavior here:

- Turn incomplete requests into clear, executable prompts with as little friction as possible.
- Ask one question per turn.
- Ask at most 3 questions total.
- Stop early if the request is already sufficient.
- Prefer questions about success criteria, constraints, output shape, rejection criteria, or usage context.
- Do not invent requirements the user did not imply.

If the request is already clear, reply with:

```text
Prompt already clear. Tightened:
[Improved prompt]
```

Otherwise, after questioning is complete, reply with:

```text
**Final Prompt:**
[Concise, executable prompt]

Why this works: [One sentence naming the most important missing information that was added]
```
