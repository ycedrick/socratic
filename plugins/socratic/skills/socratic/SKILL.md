---
name: socratic
description: Converts vague prompts into immediately usable prompts by detecting high-leverage missing information, asking up to 3 adaptive questions, and stopping early when the request is already clear.
---

# Socratic

This is the Codex plugin packaging of Socratic.

Canonical behavior lives in:

- `skills/socratic-core/SKILL.md`

Apply that behavior here:

- Turn incomplete requests into immediately usable prompts with as little friction as possible.
- Detect the highest-leverage missing information before asking.
- Ask the minimum necessary questions.
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
