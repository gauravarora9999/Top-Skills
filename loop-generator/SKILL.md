---
name: loop-generator
description: Convert any prompt or task into a loop-engineered agentic workflow with Discover/Plan/Execute/Verify/Iterate stages, a declared Stop Condition, and feedback-fed correction. Use whenever a prompt should become a loop, a self-correcting workflow, an agentic workflow, a CLAUDE.md workflow, or a slash command.
license: MIT
version: 1.0.0
compatibility: Any LLM platform that accepts a system prompt, custom instruction, or skill file (Claude, OpenAI GPT, Google Gemini, open-weights models, agent harnesses).
---

# Loop Generator

Convert any prompt into a loop-engineered workflow. The principle (Boris, Claude Code creator): stop crafting prompts, write loops. A prompt is one stateless node; a loop is stateful, multi-pass, and self-correcting. The three things a loop adds over a good prompt are a hard Verify gate, a Feedback channel that re-enters at Discover, and a Stop Condition declared upfront.

## When given a prompt, produce the loop

Read the user's prompt and extract the task. Then emit the loop in the exact template below. Do not return prose commentary unless the user asks for it — return the loop.

### Output template

```
GOAL:
<one-sentence definition of done, restated from the user's prompt>

CONTEXT:
- <relevant constraints: language, environment, file paths, data shape>
- <success constraints the output must satisfy>
- <anything the agent needs that the prompt assumed>

LOOP:

DISCOVER:
- <what the agent inspects to understand the problem space: read files, search, profile data>
- Output: <concrete artifact, e.g. list of targets>

PLAN:
- <map findings to concrete changes/actions>
- Flag any step that risks violating a constraint
- Output: ordered action list with risk annotations

EXECUTE:
- <apply the plan, one step at a time>
- Output: <the produced artifact>

VERIFY:
- <the objective check — run tests, validate schema, assert counts, lint, diff>
- If any check fails -> set Feedback = [specific failure reason] -> return to DISCOVER
- If all checks pass -> proceed to output

STOP CONDITION:
<the explicit, testable criterion that ends the loop>

VERIFIED OUTPUT:
<what ships when the Stop Condition is met, plus a summary/diff>
```

## How to fill each field well

The quality of the loop lives in **Verify** and **Stop Condition**. A loop with a vague verify step ("check it looks right") is just a prompt with extra words. Force these to be objective:

- **Verify** must be something the agent can execute and get a pass/fail from — a test suite, a schema check, a regex, a row count, a build, a linter, a diff against expected. If the task has no natural objective check, invent the cheapest proxy (e.g., "re-read output and confirm each of the N constraints is literally present") and say so.
- **Stop Condition** must be the conjunction of every Verify check. It is the contract: the loop cannot exit until this is satisfied. No "good enough."
- **Feedback** re-enters at Discover, never at Execute. The point is the agent re-analyzes with new information rather than patching blindly. State what gets passed back.
- **Goal** is one sentence. If you can't state done in one sentence, the task is underspecified — note the ambiguity in CONTEXT rather than papering over it.

If the user's prompt is missing a constraint needed for a real Verify step (e.g., no test path, no acceptance criteria), name the gap in CONTEXT as an assumption rather than silently dropping verification — a loop without verification defeats the purpose.

## Tier awareness

If the user shows you a weak prompt, you can briefly note which tier it is before converting:
- **Simple prompt** — single line, no context, no success definition. Quality is luck.
- **Good prompt** — role + constraints + output format, but still single-pass; violations caught manually.
- **Loop** — the above wrapped in Discover→Verify→Iterate with a declared Stop Condition; runs autonomously until the exit criterion is met.

The conversion target is always the loop.

## Worked example

**Input prompt:** "Refactor this function to be more readable."

**Output:**
```
GOAL:
Refactor the target function for readability without changing behaviour.

CONTEXT:
- Language: Python 3.11
- Function: [code]
- Test suite: [test file path]  <-- assumption: tests exist; if not, behaviour-equivalence cannot be verified
- Constraints: preserve signature, max 30 lines, descriptive names

LOOP:

DISCOVER:
- Read the function
- Identify: complexity hotspots, magic numbers, unclear names, nested conditionals
- Output: list of specific refactor targets

PLAN:
- Map each target to a concrete change
- Flag any change that risks altering behaviour
- Output: ordered change list with risk annotations

EXECUTE:
- Apply changes sequentially
- Output: refactored function

VERIFY:
- Run test suite against refactored function
- Check: line count <= 30, no magic numbers, no single-letter vars outside loops
- If tests fail OR any check fails -> set Feedback = [failure reason] -> return to DISCOVER
- If all pass -> proceed to output

STOP CONDITION:
All tests green + all readability checks pass

VERIFIED OUTPUT:
Refactored function + diff summary
```

## Output format note

When the user wants this loop to run autonomously inside Claude Code, tell them where it lives: a `CLAUDE.md` workflow (inherited every session) or a slash command with the verify steps baked in. The agent then runs it until the Stop Condition is met — the user writes the loop, not each pass.
