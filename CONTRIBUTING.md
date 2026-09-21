# Contributing

This repo is one skill file and its packaging. Contributions that make the skill produce better decisions are the ones that matter.

## Good contributions

- **Sharper heuristics** — a better rule for when to test vs. ship, a better traffic/boldness threshold, a better goal hierarchy for a specific kind of site (SaaS, ecommerce, lead-gen, booking).
- **Worked examples** — a real (anonymised) run through SEE → FIND → BUILD → PROVE → KEEP on a real page, with what the agent decided and why. Put them in `examples/`.
- **Tighter wording** — the skill loads into an agent's context every time it triggers. Every line costs tokens on every page build. Cuts that lose nothing are welcome.
- **Ports** — the same loop expressed for another testing tool's API, in `skills/<tool-name>/SKILL.md`.

## Less useful

- Philosophy without a rule an agent can act on.
- Adding steps to the loop. It's five on purpose — it matches the product, the docs, and the graphic.
- Precise statistical claims without a source.

## Process

1. Fork, branch, change.
2. If you changed `SKILL.md`, say in the PR what decision the agent now makes differently and why.
3. Keep the front-matter `name` and `description` intact unless the PR is specifically about triggering.

## Tone

Direct. Second person. Imperative. The skill is talking to an agent mid-task, not writing an essay.
