---
name: codex-review-loop
description: Run Codex code review as a closeout check for local dirty changes, branch/PR diffs, or a single commit. Use when the user asks for Codex review, autoreview, a second-model review, review after fixes, or when non-trivial code edits need a final advisory pass before commit, push, ship, or PR update.
---

# Codex Review Loop

Run Codex's built-in code review as a closeout check. This is `codex review`, not Guardian `auto_review` approval routing.

## Contract

- Treat review output as advisory. Never blindly apply it.
- Verify every finding by reading the real code path and adjacent files.
- Read dependency docs, source, or types when a finding depends on external behavior.
- Reject unrealistic edge cases, speculative risks, broad rewrites, and fixes that over-complicate the codebase.
- Prefer small fixes at the right ownership boundary.
- Keep going until Codex review returns no accepted actionable findings.
- If a review-triggered fix changes code, rerun focused tests and rerun Codex review.
- If rejecting a finding as intentional or not worth fixing, add a brief inline code comment only when it explains a real invariant or ownership decision future reviewers should know.
- Do not push just to review. Push only when the user requested push, ship, or PR update.

## Pick Target

Dirty local work:

```bash
codex review --uncommitted
```

Branch or PR work:

```bash
git fetch origin
codex review --base origin/main
```

Do not pass an inline prompt with `--base`; current CLI rejects `--base` plus `[PROMPT]` even though help text is ambiguous. If custom instructions are needed, run the plain base review first, then do a local/manual follow-up pass.

If an open PR exists, use its actual base:

```bash
base=$(gh pr view --json baseRefName --jq .baseRefName)
codex review --base "origin/$base"
```

Committed single change:

```bash
codex review --commit HEAD
```

## Parallel Closeout

Format first if formatting can change line locations. Then it is OK to run tests and review in parallel:

```bash
~/.codex/skills/codex-review-loop/scripts/codex-review --parallel-tests "<focused test command>"
```

Tradeoff: tests may force code changes that stale the review. If tests or review lead to code edits, rerun the affected tests and rerun review until no accepted actionable findings remain.

## Context Efficiency

Codex review is usually noisy. Default to a subagent filter when subagents are available. Ask it to run the review and return only:

- actionable findings it accepts
- findings it rejects, with one-line reason
- exact files/tests to rerun

Run inline only for tiny changes or when subagents are unavailable.

## Helper

Bundled helper:

```bash
~/.codex/skills/codex-review-loop/scripts/codex-review --help
```

The helper:

- chooses dirty `--uncommitted` first
- otherwise uses current PR base if `gh pr view` works
- otherwise uses `origin/main` for non-main branches
- writes only to stdout unless `--output` or `CODEX_REVIEW_OUTPUT` is set
- supports `--dry-run` and `--parallel-tests`

## Final Report

Include:

- review command used
- tests or proof run
- findings accepted/rejected, briefly why
- final clean review command, or why a remaining finding was consciously rejected
