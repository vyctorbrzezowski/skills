# llms

Personal LLM agent skills I use, tweak, and share with friends and colleagues.

This is not a framework or a manifesto. It is a small public shelf for prompts, skills, and agent workflows that have been useful enough to keep.

## What's here

- [`skills/codex/codex-run-to-completion`](skills/codex/codex-run-to-completion/SKILL.md): asks Codex to keep going through routine choices, waits, tests, and verification instead of stopping for avoidable confirmations.

## How to use a skill

Copy a skill directory into the skills folder for the agent you use.

For Codex:

```bash
mkdir -p ~/.codex/skills
cp -R skills/codex/codex-run-to-completion ~/.codex/skills/
```

For agents that support `SKILL.md`, the important file is always inside the skill directory.

## Shape

```text
skills/
  codex/
    skill-name/
      SKILL.md
      agents/
        openai.yaml
  generic/
    skill-name/
      SKILL.md
```

Some skills may include scripts, references, or templates when that makes the skill easier to reuse. Most should stay small.

## Notes

These are personal working tools, so names and behavior may change. If you are using one, pin it or copy it into your own setup.
