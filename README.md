# Top-Skills

A collection of portable, vendor-neutral AI skills. Each skill is a single self-contained
`SKILL.md` (plain Markdown + YAML frontmatter) that works on any LLM platform that accepts a system
prompt, custom instruction, or project/skill file — Claude, OpenAI GPT, Google Gemini, open-weights
models, and agent harnesses. Download a skill, drop it into your platform, and go.

## Skills

| Skill | What it does | Status |
|-------|--------------|--------|
| [mega-meta-prompt](./mega-meta-prompt) | Turns any raw prompt into a hardened, production-grade prompt using merged Anthropic + OpenAI + Google best practices | Stable (v1.0.0) |

## How these skills are built

Every skill follows the same portable contract:

- **One file does the work.** `SKILL.md` carries a YAML frontmatter (`name`, `description`,
  `license`, `version`, `compatibility`) followed by the instructions. The frontmatter is read by
  skill-aware runtimes; the body works as a plain system prompt everywhere else.
- **Vendor lock is quarantined.** Anything model-specific lives in a single clearly-labelled
  section you apply only for your target model. The core is model-agnostic.
- **No tools required.** Skills function with nothing but a text box, and light up further when
  tools or an agent loop are present.

## Install (any skill, any platform)

Open the skill's folder, read its `README.md` for platform-specific steps, then use whichever fits:

- **Claude (Cowork / Claude Code):** install the skill folder or its `.skill` package.
- **claude.ai Projects:** paste the `SKILL.md` body into custom instructions.
- **OpenAI Custom GPT / API:** paste the body into Instructions, or send it as the `system` message.
- **Google Gemini Gems / API:** paste the body into the Gem, or pass it as `system_instruction`.
- **Agent harnesses (OpenClaw etc.):** load `SKILL.md` as the agent's system prompt.
- **Copilot / Cursor / Windsurf:** add the body to your repo rules file.
- **Any other LLM:** paste the body at the top of the conversation.

## Contributing

New skills should be self-contained, portable, and ship with their own `README.md` and license.
Keep model-specific advice isolated so the core stays vendor-neutral.

## License

MIT — see [LICENSE](./LICENSE). Each skill also carries its own license file.

---

Maintained by [Gaurav Arora](https://github.com/gauravarora9999).
