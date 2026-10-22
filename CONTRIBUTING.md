# Contributing

This repo is two agent skills, the reference files they share, and their packaging. Contributions that make an agent produce better decisions are the ones that matter. See `docs/how-the-skills-fit.md` for which skill owns what.

## Good contributions

- **Sharper heuristics** — a better rule for when to test vs. ship, a better traffic/boldness rule, a better goal hierarchy for a specific kind of site (SaaS, ecommerce, lead-gen, booking).
- **Worked examples** — a real (anonymised) run through SEE → FIND → BUILD → PROVE → KEEP on a real page, with what the agent decided and why. Put them in `skills/website-optimisation-loop/references/`, labelled as real or hypothetical.
- **Tighter wording** — `prove-website-changes` loads into an agent's context every time it triggers. Every line costs tokens on every page build. Cuts that lose nothing are welcome.
- **Platform notes** — how to run the loop on another testing tool, as a reference file next to `wordpress.md` and `cloud.md`. Explain when and why; leave parameters to the tool's own MCP descriptions.

## Less useful

- Philosophy without a rule an agent can act on.
- Adding steps to the loop. It's five on purpose — it matches the product, the docs, and the graphic.
- Precise statistical claims without a source.

## Process

1. Fork, branch, change.
2. If you changed a `SKILL.md`, say in the PR what decision the agent now makes differently and why.
3. If you changed a safety rule that appears in both skills, change it in both. Keep the front-matter `name` and `description` intact unless the PR is specifically about triggering.

## Tone

Direct. Second person. Imperative. The skill is talking to an agent mid-task, not writing an essay.
