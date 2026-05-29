# skills

Personal agent skills I use, tweak, and share with friends and colleagues.

This is not a framework or a manifesto. It is a small public shelf for prompts, skills, and agent workflows that are useful enough to reuse.

The goal is simple: keep the agent moving in the style I want, make good workflows easy to copy, and avoid re-explaining the same operating rules every time.

## What's here

- [`session-brief`](skills/session-brief/SKILL.md): dumps a compact session context summary directly in chat.
- [`session-migrate`](skills/session-migrate/SKILL.md): creates a pasteable handoff prompt for continuing the session in a new chat.
- [`write-better-tests`](skills/write-better-tests/SKILL.md): designs and reviews behavior-first tests that catch real regressions instead of only passing the current implementation.
- [`codex-run-to-completion`](skills/codex/codex-run-to-completion/SKILL.md): asks Codex to keep going through routine choices, waits, tests, and verification instead of stopping for avoidable confirmations.
- [`playground-oracle`](skills/codex/playground-oracle/SKILL.md): consults Opus 4.7 and GPT-5.5-Pro in the Vercel AI SDK Playground as a cheap outside oracle for hard calls, proof-checking, plan review, and adversarial reasoning.
- [`codex-review-loop`](skills/codex/codex-review-loop/SKILL.md): runs Codex review as an advisory closeout loop, verifies findings, fixes the real ones, and repeats until clean.
- [`codex-session-image-scrubber`](skills/codex/codex-session-image-scrubber/SKILL.md): removes heavy embedded image/base64 payloads from local Codex session logs while preserving conversation text.

## Install

With `skills.sh`:

```bash
npx skills@latest add vyctorbrzezowski/skills
```

Or copy one skill manually.

For Codex:

```bash
mkdir -p ~/.codex/skills
cp -R skills/session-brief skills/session-migrate skills/write-better-tests ~/.codex/skills/
cp -R skills/codex/* ~/.codex/skills/
```

For agents that support `SKILL.md`, the important file is always inside the skill directory.

## Reference

### General

Portable skills that only depend on the conversation context.

- [`session-brief`](skills/session-brief/SKILL.md): produce a compact context dump in chat for the current session. Use it when the chat is bloated, slow, or the user wants the current state without opening files.
- [`session-migrate`](skills/session-migrate/SKILL.md): produce a copyable prompt for a new chat, with the goal, completed work, remaining work, current state, files, constraints, and suggested skills.
- [`write-better-tests`](skills/write-better-tests/SKILL.md): design or review tests around real product/code contracts, require red-green proof when practical, and reject convenient mocks, fixtures, snapshots, and assertions that would not catch the regression.

### Codex

Skills that depend on Codex behavior or OpenAI agent metadata.

- [`codex-run-to-completion`](skills/codex/codex-run-to-completion/SKILL.md): run a task end to end without stopping for avoidable confirmations, including long waits, terminal polling, verification, and concise closeout.
- [`playground-oracle`](skills/codex/playground-oracle/SKILL.md): ask Opus 4.7 and GPT-5.5-Pro in the AI SDK Playground for a second opinion without burning Codex quota. Useful for architecture calls, tricky reviews, plan stress-tests, proof-checking, and reasoning checks. It is inspired by [`steipete/oracle`](https://github.com/steipete/oracle), but aimed at a lower-cost browser-playground workflow.
- [`codex-review-loop`](skills/codex/codex-review-loop/SKILL.md): run `codex review`, treat the output as advisory, verify each finding locally, apply only real fixes, rerun tests, and review again.
- [`codex-session-image-scrubber`](skills/codex/codex-session-image-scrubber/SKILL.md): locate oversized Codex session JSONL files, back them up, replace embedded image payloads with small placeholders, and validate the cleaned history.

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
