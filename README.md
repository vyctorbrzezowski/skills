# skills

Personal LLM agent skills I use, tweak, and share with friends and colleagues.

This is not a framework or a manifesto. It is a small public shelf for prompts, skills, and agent workflows that are useful enough to reuse.

The goal is simple: keep the agent moving in the style I want, make good workflows easy to copy, and avoid re-explaining the same operating rules every time.

## What's here

- [`skills/codex/codex-run-to-completion`](skills/codex/codex-run-to-completion/SKILL.md): asks Codex to keep going through routine choices, waits, tests, and verification instead of stopping for avoidable confirmations.

## Install

With `skills.sh`:

```bash
npx skills@latest add vyctorbrzezowski/skills
```

Or copy one skill manually.

For Codex:

```bash
mkdir -p ~/.codex/skills
cp -R skills/codex/codex-run-to-completion ~/.codex/skills/
```

For agents that support `SKILL.md`, the important file is always inside the skill directory.

## Reference

### Codex

Skills that depend on Codex behavior or OpenAI agent metadata.

- [`codex-run-to-completion`](skills/codex/codex-run-to-completion/SKILL.md): run a task end to end without stopping for avoidable confirmations, including long waits, terminal polling, verification, and concise closeout.

## Shape

```text
skills/
  skill-name/
    SKILL.md
  codex/
    skill-name/
      SKILL.md
      agents/
        openai.yaml
```

Some skills may include scripts, references, or templates when that makes the skill easier to reuse. Most should stay small.

## Notes

These are personal working tools, so names and behavior may change. If you are using one, pin it or copy it into your own setup.
