---
name: playground-oracle
description: Consult the AI SDK Playground as a frontier-model oracle for hard engineering, code review, architecture, PR, debugging, product, thesis, proof-check, and adversarial reasoning decisions. Use when Codex is uncertain, the problem is high-impact or subtle, a second frontier opinion would materially reduce risk, the user asks to proof-check or stress-test a plan or implementation with the playground, or competing approaches need adversarial analysis before action.
---

# Playground Oracle

Use the AI SDK Playground as an outside judgment surface, not as a replacement for local evidence. The default playground setup has two synchronized model panels, currently GPT-5.5-Pro and Opus 4.7; typing in either input and sending once submits to both.

Use the built-in Codex `@Browser` plugin UI for this skill. The Browser-backed Chrome surface is allowed when the user wants Chrome or needs an existing browser session; the Codex in-app browser is preferred for clean deep-link tests. Do not use Playwright-driven selectors, standalone Playwright automation, the `@Computer` plugin, or Computer Use for the playground workflow.

## Core Workflow

1. Decide whether an oracle call is worth the latency.
   Use it for difficult, ambiguous, risky, strategic, or high-leverage questions. Skip it for routine edits, obvious bugs, simple command output, or tasks where local tests and source inspection are enough.

2. Prepare a compact prompt.
   Include the actual question, the relevant constraints, the local evidence already gathered, and the decision you need help making. Prefer focused snippets, diffs, errors, and repo facts over broad dumps. Do not send secrets, private keys, credentials, tokens, customer data, or unnecessary proprietary content.

3. Open `https://ai-sdk.dev/playground/` with the built-in Codex `@Browser` plugin UI.
   Use Browser-backed Chrome when the user asks for Chrome or needs an existing browser session. Use the Codex in-app browser for clean deep-link tests. Confirm the two model panels are present and that the shared input behavior is active. Do not use Playwright or Computer Use.
   To force a model set in a clean session, use a path deep link:

   ```text
   https://ai-sdk.dev/playground/<model-id-1>,<model-id-2>
   ```

   Default oracle pair:

   ```text
   https://ai-sdk.dev/playground/openai:gpt-5.5-pro,anthropic:claude-opus-4-7
   ```

   Use GPT-5.5-Pro and Opus 4.7 as the default pair for general work and hard specialized questions. Do not rotate to extra models by topic unless the user explicitly asks for a different comparison. Verify the visible model labels before sending.

   The Codex in-app browser does not carry the user's Chrome `localStorage`, so `/playground/model-a,model-b` is applied directly.

4. Send exactly one message.
   Paste the entire prompt into one input as a single message, preserving internal line breaks. Do not send the context in separate messages or type it in chunks that may submit early. Because the fields are synchronized, one complete paste and one submit sends the same full message to both models. Opus usually returns first; GPT-5.5-Pro may take longer due to thinking. Do not synthesize until both have either answered or clearly failed/timed out.

5. Compare, then decide.
   Extract the strongest arguments from each answer. Look for agreement, disagreement, missing assumptions, weak reasoning, and concrete checks suggested by the models. Treat confident claims as hypotheses unless they match local source, tests, or user-provided constraints.

6. Bring back a useful synthesis.
   Report the practical conclusion, where the models agreed, where they diverged, and what you will do next. Keep the user-facing answer short unless the user asked for deep analysis.

## Prompt Shape

Use this structure when the question is complex:

```text
We are using this playground as an oracle for a coding/product decision.

Context:
- Repo/task:
- Current evidence:
- Constraints:
- Proposed plan or implementation:

Question:
Evaluate this. What is likely wrong, missing, risky, or overcomplicated? If you disagree with the plan, give the better path and the concrete checks that would prove it.
```

For implementation review:

```text
Review this change as a senior engineer. Focus on correctness, hidden regressions, edge cases, tests, and whether the design is simpler than the alternatives. Do not nitpick style unless it affects behavior.

Relevant diff/snippets:
...
```

For architecture or thesis checks:

```text
Stress-test this thesis. Identify the strongest counterargument, the condition under which it fails, and what evidence would change your mind.

Thesis:
...
Evidence:
...
```

## Operating Rules

- Ground the oracle with local facts first. Inspect the code, errors, docs, or PR state before asking whenever that context is available.
- Use the built-in Codex `@Browser` UI for playground access. Do not use Playwright, standalone browser automation, `@Computer`, or Computer Use for this workflow.
- Ask the oracle for judgment, not generic explanation. The prompt should request a decision, critique, trade-off, or verification path.
- Send the full oracle prompt as one playground message. Never split context, bullets, question, or follow-up into multiple playground messages unless the user explicitly asks for a follow-up after the first oracle response.
- Use both answers independently. Do not stop after the faster model unless the task is explicitly time-sensitive.
- Prefer adversarial prompts for plans and reviews: ask what is wrong, missing, risky, or unnecessarily complex.
- Do not use the playground as a live fact-checking surface. It has no tool calls in this workflow. Use it to proof-check reasoning, stress-test plans, surface counterarguments, and review assumptions; use official docs, local source, tests, or the browser for factual verification.
- If the models disagree, preserve the disagreement in the synthesis and resolve it with evidence, not vibes.
- If both answers are generic, tighten the prompt with concrete snippets and ask again only when the added latency is justified.
- Never paste secrets or sensitive data into the playground.

## Output Pattern

When reporting back, use this shape unless the user requested another format:

```text
Oracle check: both models agreed that ...

GPT-5.5-Pro emphasized ...
Opus 4.7 emphasized ...

My decision: ...
Next check: ...
```

For small confirmations, compress it to one paragraph.
