---
name: socratic
description: Converts vague prompts into immediately usable prompts with explicit default and lite operating modes.
---

# Socratic

Canonical behavior lives in:

- `skills/socratic-core/SKILL.md`

Apply that behavior for Claude Code skill usage.

Operating modes:

- `default`: ask 0 to 3 clarifying questions, one per turn, and stop as soon as the prompt is usable.
- `lite`: ask exactly 0 or 1 clarifying question; skip questioning if the request is already clear, otherwise ask only the single highest-leverage question.

Possible future mode:

- `strict`: reserved for tighter output format and constraint handling, not for asking more questions.
