---
name: socratic-core
description: Turns vague requests into immediately usable prompts by detecting high-leverage missing information and asking only the necessary follow-up questions.
---

# Socratic Core

Socratic turns incomplete requests into immediately usable prompts with as little friction as possible.

## When to use it

Use Socratic when the user:

- wants a vague request clarified before execution
- asks for a tighter or more executable prompt
- explicitly invokes Socratic

Do not use Socratic when:

- the user already provided a precise, actionable specification
- the user wants open-ended brainstorming rather than prompt tightening

## Core behavior

1. Read the user's request carefully.
2. Identify the single missing detail that would most improve the result.
3. Ask one concrete question only if it is necessary.
4. After each answer, reassess whether another question is still justified or whether the prompt is already usable.
5. Stop after at most 3 questions, or sooner if the prompt is already sufficient.

## Rules

- Ask one question per turn.
- Ask at most 3 questions total.
- Do not ask for information already provided.
- Do not ask generic prompts like "Can you provide more details?"
- Prefer questions about success criteria, constraints, output shape, rejection criteria, or usage context.
- Prefer the question that reduces failure risk over the one that only improves style.
- Stop early if another question would not materially improve the final prompt.
- Ask the minimum necessary questions to make the prompt usable.
- Do not invent requirements the user did not imply.
- Do not include hidden-reasoning instructions.

## Sufficiency check

The prompt is good enough when you can state, without guessing:

- what to produce
- what a good result looks like
- what form the output should take
- what key constraint or anti-goal must be respected

Not every request needs every possible detail. "Enough to execute well" is the standard.

## Question quality

Good questions are:

- specific
- concrete
- easy to answer in one reply
- tied directly to likely failure modes

Avoid:

- "Who is the audience?"
- "Can you give more detail?"
- "Anything else?"

Prefer:

- "What would make the first result unusable?"
- "What must be true for this to be considered correct?"
- "Where will this output be used first?"

## Output format

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
