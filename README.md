# Socratic

Socratic turns vague requests into immediately usable prompts by detecting the missing information that matters most, asking the minimum necessary questions, and stopping as soon as the prompt is ready to run.

## Behavior

Socratic is intentionally not always-on. It is a manual clarification tool.

Core capability:

- Detect the highest-leverage missing information
- Ask the minimum necessary questions
- Produce a prompt that is short, specific, and immediately usable
- Prioritize different unknowns for common prompt families such as coding, design, writing, planning, and automation

Operating modes:

- `default`: ask 0 to 3 questions, one per turn, and stop as soon as the prompt is usable
- `lite`: ask exactly 0 or 1 question; skip questioning if already clear, otherwise ask only the single highest-leverage question
- `strict`: reserved for a future mode focused on tighter output format and constraint handling, not extra questioning

Core rules:

- Ask at most 3 questions
- Ask one question per turn
- Stop early if the request is already clear
- Avoid generic “tell me more” questions
- Prefer questions about success criteria, constraints, output shape, rejection criteria, or usage context
- Return a final prompt that is short, specific, and immediately usable

Domain-aware questioning:

- Socratic remains general-purpose, but it silently infers common prompt families and adjusts question priorities.
- In v1, the core domains are `coding`, `design`, `writing`, `planning`, and `automation`.
- If domain confidence is low or mixed, Socratic falls back to the general-purpose question framework instead of forcing a weak guess.

## Usage Examples

### 1. Vague Feature Request
**User:** `/soc Add auth to my Express app.`
**Socratic:** "Which authentication strategy do you prefer (e.g., Passport.js with JWT, NextAuth, or a custom session-based approach)?"
**User:** "Passport with JWT."
**Socratic:**
> **Final Prompt:**
> Implement authentication in the Express application using Passport.js and JSON Web Tokens (JWT). Include routes for user registration, login, and a protected profile route. Use a local strategy for initial authentication and ensure passwords are hashed before being stored in the database.
>
> Why this works: Specifying the exact library (Passport.js) and strategy (JWT) removes the primary ambiguity from the initial request.

### 2. Request for a Tighter Prompt
**User:** `/soc Make this prompt better: 'Write a script to scrape a website.'`
**Socratic:** "What is the target website or type of data you need to extract (e.g., product prices from an e-commerce site, news headlines), and which language/library should the script use?"
**User:** "Python with BeautifulSoup to get stock prices from a table."
**Socratic:**
> **Final Prompt:**
> Write a Python script using the BeautifulSoup library to scrape stock price data from a HTML table on a specified financial news website. The script should handle basic error checking for network requests and output the scraped data into a structured CSV format.
>
> Why this works: Adding the specific library, target data, and output format transforms a generic request into an actionable technical task.

### 3. Explicit Socratic Invocation (Already Clear)
**User:** `/soc Write a React component for a primary button that accepts 'label' and 'onClick' props, styled with vanilla CSS.`
**Socratic:**
> Prompt already clear. Tightened:
> Create a functional React component named `PrimaryButton` that accepts `label` (string) and `onClick` (function) as props. Include a companion vanilla CSS file for styling, ensuring the button has a consistent padding, font size, and a hover state transition.

## Compatibility Matrix

| Tool | Packaging | Install Surface | Invocation Model |
| :--- | :--- | :--- | :--- |
| Claude Code | Project or user slash commands | `.claude/commands/` | Manual slash commands |
| Codex | Local plugin + skill | `plugins/socratic/` and `.agents/plugins/marketplace.json` | Manual plugin/skill use |
| Gemini CLI | Repo-root extension | `gemini-extension.json`, `GEMINI.md`, `commands/` | Manual extension commands |

## Install

### Claude Code

Project-local install:

```bash
mkdir -p .claude/commands
cp .claude/commands/soc.md .claude/commands/soc-lite.md .claude/commands/socratic.md /path/to/your-project/.claude/commands/
```

User-level install:

```bash
mkdir -p ~/.claude/commands
cp .claude/commands/soc.md .claude/commands/soc-lite.md .claude/commands/socratic.md ~/.claude/commands/
```

Commands:

| Command | Purpose |
| :--- | :--- |
| `/soc` | Default mode with 0 to 3 clarifying questions |
| `/socratic` | Alias for `/soc` |
| `/soc-lite` | Lite mode with exactly 0 or 1 clarifying question |

### Codex

Repo-local plugin install:

1. Open Codex in this repo.
2. Open `/plugins`.
3. Search for `Socratic`.
4. Install the local plugin.

Direct skill install:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills/socratic"
cp .codex/skills/socratic/SKILL.md "${CODEX_HOME:-$HOME/.codex}/skills/socratic/"
```

Notes:

- Socratic is manual-invocation by design.
- This repo does not auto-enable a `SessionStart` hook for Socratic.

### Gemini CLI

Install from the repo root:

```bash
gemini extensions install <repo-url-or-local-path>
```

For local development:

```bash
gemini extensions link .
```

Commands after restart:

| Command | Purpose |
| :--- | :--- |
| `/soc` | Default mode with 0 to 3 clarifying questions |
| `/socratic` | Alias for `/soc` |
| `/soc:lite` | Lite mode with exactly 0 or 1 clarifying question |
| `/socratic:lite` | Alias for `/soc:lite` |

## Shared Core

The canonical behavior lives in:

- `skills/socratic-core/SKILL.md`

Agent-specific wrappers should stay thin and point back to that core behavior whenever the host tool supports file references.

## Evaluation Set

The current evaluation set contains 14 handcrafted prompt cases across coding, design, writing, planning, analysis, research, automation, frontend, and marketing.

These numbers describe the intended interaction profile of the current eval set. They are design targets, not measured production performance.

| Metric | Result |
| :--- | :--- |
| Total benchmark cases | 14 |
| Cases expected to finish in 0 questions | 5 |
| Cases expected to finish in 1 question | 6 |
| Cases expected to finish in 2 questions | 3 |
| Cases expected to finish in 3 questions | 0 |
| Median ideal question count | 1 |
| Mean ideal question count | 0.86 |

Target question-count distribution:

| Ideal questions | Cases | Share |
| :---: | :---: | :---: |
| 0 | 5 | 36% |
| 1 | 6 | 43% |
| 2 | 3 | 21% |
| 3 | 0 | 0% |

This is the intended product shape: most vague prompts should be improved in 1 question, some need 2, and already-clear prompts should skip questioning entirely.

## License

MIT
