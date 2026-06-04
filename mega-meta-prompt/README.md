# Mega Meta Prompt

A portable, vendor-neutral prompt-engineering skill. It turns any raw prompt or task description
into a hardened, production-grade prompt. The rule set merges the official prompting guides of
Anthropic (Claude), OpenAI (GPT), and Google (Gemini), deduplicated into one non-contradictory
framework. Model-specific advice is quarantined in a single "Model-family notes" section that you
apply only for your target model.

The whole skill is one file: `SKILL.md`. It is plain Markdown with a YAML header, so it works
anywhere you can paste a system prompt, custom instruction, or project file.

## What's in this package

- `SKILL.md` — the skill itself (frontmatter + framework). This is the only file you need.
- `README.md` — this file.
- `LICENSE` — MIT.

## Install / use by platform

### Claude — Cowork or Claude Code (skill)
1. Zip this folder so it contains `SKILL.md` at the root (or use the provided `.skill` file).
2. In Cowork, use the "Save skill" / install action on the `.skill` file. In Claude Code, drop the
   folder into your skills directory (e.g. `~/.claude/skills/mega-meta-prompt/`).
3. Invoke by asking anything like "make this prompt production-grade".

### Claude — claude.ai Projects
Create a Project, open **Project knowledge / Custom instructions**, and paste the body of
`SKILL.md` (everything below the `---` frontmatter). The frontmatter is optional outside the skill
runtime.

### OpenAI — Custom GPT or API system prompt
- **Custom GPT**: in the GPT builder, paste the body of `SKILL.md` into **Instructions**.
- **API**: send the body of `SKILL.md` as the `system` message (Chat Completions) or as
  `instructions` (Responses API). Keep the `## Model-family notes → GPT` section; you may delete
  the others to save tokens.

### Google — Gemini Gems or API system instruction
- **Gem**: paste the body of `SKILL.md` into the Gem's instructions.
- **API**: pass the body as `system_instruction`. Keep temperature at the platform default (1.0 on
  recent Gemini); lowering it can degrade reasoning. Keep the `## Model-family notes → Gemini`
  section.

### OpenClaw / other agent harnesses
Load `SKILL.md` as the agent's system prompt or as a tool/skill definition. Keep the
"Tool & agent rules" and "AGENT REASONING GATE" sections — they are written for agent loops.

### GitHub Copilot / Cursor / Windsurf and similar
Add the body of `SKILL.md` to your repo rules file (`.github/copilot-instructions.md`,
`.cursorrules`, `.windsurfrules`, etc.) or paste it into the chat as context before asking for a
prompt.

### Generic fallback (any LLM)
Paste the body of `SKILL.md` at the top of your conversation, then state your prompt-building
request underneath. The framework needs no tools to function.

## How to use it once loaded

Give it a raw prompt, a half-formed instruction, or just a goal. It will (optionally) ask for any
material missing inputs, then return:

1. **CLARIFYING INPUTS NEEDED** — only if something material is missing.
2. **GOLD PROMPT** — the rewritten, paste-ready prompt.
3. **RUNTIME CONFIG** — model/temperature/tooling settings, when the target is an API or agent.
4. **RATIONALE** — short mapping of each design choice to its rule.

## Maintenance note

The "Model-family notes" section decays fastest because vendors change defaults, limits, and APIs.
Re-verify that section against current vendor documentation before high-stakes use. The core
construction rules are stable.

## License

MIT. Free to use, modify, and redistribute. See `LICENSE`.
