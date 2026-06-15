# Loop Generator

A portable, vendor-neutral skill that converts any prompt or task into a loop-engineered agentic workflow. Instead of crafting a single stateless prompt, it produces a stateful, multi-pass, self-correcting loop with five stages — Discover, Plan, Execute, Verify, Iterate — a hard Verify gate, a Feedback channel that re-enters at Discover, and a Stop Condition declared upfront.

The whole skill is one file: SKILL.md. It is plain Markdown with a YAML header, so it works anywhere you can paste a system prompt, custom instruction, or project file.

## What's in this package

- `SKILL.md` — the skill itself (frontmatter + method). This is the only file you need.
- `README.md` — this file.
- `LICENSE` — MIT.

## Why a loop beats a prompt

A prompt is one stateless pass; quality is luck. A loop adds three things a good prompt lacks:

1. A hard **Verify** gate — an objective pass/fail check (tests, schema, lint, diff, row count), not "looks right".
2. A **Feedback** channel that re-enters at **Discover** so the agent re-analyzes instead of patching blindly.
3. A **Stop Condition** declared upfront — the conjunction of every Verify check, with no "good enough" exit.

## Install / use by platform

### Claude — Cowork or Claude Code (skill)

Zip this folder so it contains `SKILL.md` at the root (or use a `.skill` package). In Cowork, use the "Save skill" / install action. In Claude Code, drop the folder into your skills directory (e.g. `~/.claude/skills/loop-generator/`). Invoke by asking anything like "turn this prompt into a loop".

### Claude — claude.ai Projects

Create a Project, open Custom instructions, and paste the body of `SKILL.md` (everything below the frontmatter).

### OpenAI — Custom GPT or API system prompt

Custom GPT: paste the body of `SKILL.md` into Instructions. API: send the body as the system message (Chat Completions) or as instructions (Responses API).

### Google — Gemini Gems or API system instruction

Gem: paste the body into the Gem's instructions. API: pass the body as `system_instruction`.

### Agent harnesses (OpenClaw, etc.)

Load `SKILL.md` as the agent's system prompt. The Verify and Stop Condition stages are written for autonomous loops.

### GitHub Copilot / Cursor / Windsurf

Add the body of `SKILL.md` to your repo rules file (`.github/copilot-instructions.md`, `.cursorrules`, `.windsurfrules`).

### Generic fallback (any LLM)

Paste the body of `SKILL.md` at the top of the conversation, then give it your prompt to convert. No tools required.

## How to use it once loaded

Give it a raw prompt, a half-formed instruction, or just a goal. It returns the loop in a fixed template: GOAL, CONTEXT, LOOP (Discover / Plan / Execute / Verify), STOP CONDITION, and VERIFIED OUTPUT. The quality lives in the Verify and Stop Condition fields — both are forced to be objective and testable.

## License

MIT. Free to use, modify, and redistribute. See LICENSE.
