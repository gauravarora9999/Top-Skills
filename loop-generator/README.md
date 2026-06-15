# Loop Generator

> Stop writing prompts. Start writing loops.

A portable, vendor-neutral skill that converts any prompt or task into a **loop-engineered agentic workflow** — Discover → Plan → Execute → Verify → Iterate, with a hard verify gate and a stop condition declared upfront. The whole skill is one file: [`SKILL.md`](./SKILL.md).

---

## Loop Engineering: stop writing prompts, start writing loops

The person who built Anthropic's flagship coding agent doesn't write prompts anymore. Boris Cherny, the creator of Claude Code, described his current workflow bluntly: he doesn't prompt the model directly — he has loops running that prompt the model, read the result, decide what to do next, and iterate, all without a human in the conversation. Google engineer Addy Osmani gave the pattern a name that stuck: **loop engineering**.

If you spent the last two years getting good at prompt engineering, this is not bad news. It's a promotion. The prompt didn't leave the room — it got wrapped in something more reliable.

### The unit of work keeps moving up

The unit of work moved from the keystroke (you type every line), to the prompt (you describe the outcome and the model writes the lines), to the loop (you describe the outcome **and the test for done**, and the system writes, checks, and rewrites until it passes).

A prompt is a single, stateless shot. You craft the perfect instruction, fire it once, and hope the output is right. When it isn't, you notice, tweak the wording, and fire again — with you as the feedback mechanism. A loop makes the system stateful and self-correcting. It runs multiple passes, checks its own work against an objective gate, and only exits when that gate is satisfied.

### What a loop actually adds

A loop is not just "a prompt with extra words." Compared to even a very good prompt, it adds three things a single shot structurally cannot have:

1. **A hard Verify gate.** An objective pass/fail check the system runs on its own output — a test suite, a schema validation, a linter, a row count, a diff against expected. Not "does this look right," but "did the tests pass." Feedback is what makes a loop trustworthy; a loop with nothing to push back is just the agent agreeing with itself.
2. **A Feedback channel that re-enters at the start.** When verification fails, the specific reason is fed back in and the system re-analyzes the problem rather than blindly patching the last attempt.
3. **A Stop Condition declared upfront.** The exit criterion — the conjunction of every Verify check — is fixed before the work begins. There is no "good enough." The loop cannot terminate until the condition is met.

### A concrete demo: the same task, three ways

Take a deliberately simple task: *refactor this function.*

**As a weak prompt:** "Refactor this function." You get *a* result. Whether it's the result you wanted is luck.

**As a good prompt:** "Refactor this function for readability — preserve the signature, keep it under 30 lines, use descriptive names." Much better. You've added role, constraints, and a definition of done. But it still runs exactly once. If the refactor quietly breaks a test, nothing catches it except you, later.

**As a loop:**

```
GOAL: Refactor the function for readability without changing behaviour.

LOOP:
  DISCOVER: read the function; list complexity hotspots, unclear names, nested conditionals.
  PLAN:     map each to a concrete change; flag anything that risks altering behaviour.
  EXECUTE:  apply the changes.
  VERIFY:   run the test suite; check line count <= 30, no magic numbers.
            if anything fails -> feed the reason back -> return to DISCOVER.
            if everything passes -> exit.

STOP CONDITION: all tests green AND all readability checks pass.
```

Same model, same task. The difference is the contract. The prompt *hopes* the output is correct; the loop *cannot finish* until the output is provably correct. That is the whole game — you stop hoping and start guaranteeing.

### When to use a loop — and when not to

Loops earn their cost when the task has an objective check and you'd otherwise babysit it: code with a test suite, data transforms with a schema, anything you'd run repeatedly until it's right. For a one-off question, a creative draft, or a task with no measurable "done," a good prompt is still the right tool — wrapping it in a loop with a fake verify step just adds ceremony. If a task has no natural objective check, you either invent the cheapest real proxy or you admit it doesn't need a loop.

---

## What this skill does

Hand `loop-generator` any prompt or task and it returns a fully-formed loop: a one-sentence **GOAL**, the **CONTEXT** and constraints, the **DISCOVER → PLAN → EXECUTE → VERIFY** stages, a Feedback path that re-enters at Discover, and an explicit, testable **STOP CONDITION**. Its design pressure sits on the two fields that matter most — Verify and Stop Condition — forcing both to be objective rather than vague.

## What's in this package

- [`SKILL.md`](./SKILL.md) — the skill itself (frontmatter + method). This is the only file you need.
- `README.md` — this file.
- `LICENSE` — MIT.

## Install / use by platform

The skill is a single, portable `SKILL.md` (plain Markdown + YAML frontmatter), so it works anywhere you can paste a system prompt.

- **Claude (Cowork / Claude Code):** install the skill folder or its `.skill` package; in Claude Code drop it into `~/.claude/skills/loop-generator/`. Invoke with anything like "turn this prompt into a loop."
- **claude.ai Projects:** paste the body of `SKILL.md` (below the frontmatter) into Custom instructions.
- **OpenAI Custom GPT / API:** paste the body into Instructions, or send it as the `system` message.
- **Google Gemini Gems / API:** paste the body into the Gem, or pass it as `system_instruction`.
- **Agent harnesses (OpenClaw, etc.):** load `SKILL.md` as the agent's system prompt.
- **Copilot / Cursor / Windsurf:** add the body to your repo rules file.
- **Any other LLM:** paste the body at the top of the conversation.

## Get it

Clone or download the repo, or copy this folder:

```
git clone https://github.com/gauravarora9999/Top-Skills.git
```

Anyone can re-derive or run the skill from `github.com/gauravarora9999/Top-Skills/tree/main/loop-generator`.

## License

MIT. Free to use, modify, and redistribute. See [LICENSE](./LICENSE).
