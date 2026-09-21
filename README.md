<div align="center">

# human-web-feedback-loop

**AI has opinions. Humans have proof.**

An agent skill that stops AI-built websites from shipping guesses — every material change gets split tested on real visitors, winners become the new baseline, and the agent learns instead of overwriting.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Skill](https://img.shields.io/badge/type-agent%20skill-38d7d3)](skills/ab-split-test/SKILL.md)
[![Works with](https://img.shields.io/badge/works%20with-Claude%20Code%20%7C%20Codex%20%7C%20Cursor%20%7C%20any%20MCP%20client-365187)](#install)
[![Powered by](https://img.shields.io/badge/powered%20by-AB%20Split%20Test-365187)](https://absplittest.com)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

[Install](#install) · [Why](#why-this-exists) · [The loop](#the-proof-loop) · [What it does](#what-the-agent-actually-does) · [Example](#example) · [FAQ](#faq)

</div>

---

## TL;DR

```bash
# Claude Code / any skills-folder agent
git clone https://github.com/SiteSpot/human-web-feedback-loop.git
cp -r human-web-feedback-loop/skills/ab-split-test ~/.claude/skills/
```

Then point your agent at a site running [AB Split Test](https://absplittest.com) (free tier works) and build like normal. The next time it changes a headline, an offer, a checkout or a landing page, it will **test the change on real traffic instead of publishing it**, wait for a statistically real answer, keep the winner, and record what it learned.

Thirty seconds to install. Every page your agent builds from then on gets proven before it gets trusted.

---

## Why this exists

Agents are now writing the copy, building the pages, restructuring the funnels. They do it confidently. They do it fast. And then they set it free on the web — with no idea whether it works.

That's not an AI problem. It's a *feedback* problem. A model can be an expert at copywriting and page speed and still be wrong for *your* audience, because every audience is different. Nobody — human or model — gets it right the first time. The only way to know what works on your page with your people is to try different things on your page with your people, and let them tell you.

Split testing is that feedback loop. This skill wires it into your agent so the loop closes automatically.

| Without this skill | With this skill |
|---|---|
| Agent rewrites the hero and publishes it | Agent rewrites the hero and **tests it against the original** |
| "Looks better" is the success criterion | **Completed bookings / revenue** is the success criterion |
| Yesterday's guess replaced by today's guess | Winner becomes the baseline; **next test starts from proof** |
| Losing changes go live and quietly cost money | Losing variations **pause and roll back**; the loss is recorded as a learned constraint |
| Agent optimises clicks because clicks are easy | Agent measures the goal **closest to revenue** it can reach |

---

## The Proof Loop

Everything in the skill hangs off one five-step loop:

```
        ┌──────────────────────────────────────────────┐
        │                                              │
        ▼                                              │
      SEE ──────► FIND ──────► BUILD ──────► PROVE ──► KEEP
   heatmaps,    highest-ICE   one hypothesis,  real       winner = new
   replays,     hypothesis,   right mode,      visitors,  baseline; loss =
   analytics    bold enough   goal ≈ revenue   Bayesian   learned constraint
```

1. **SEE** — read heatmaps, scroll maps, replays and page analytics *before* proposing anything. Let behaviour choose the lever. Nobody reaches the CTA? Test position, not wording.
2. **FIND** — generate hypotheses, score them with ICE (Impact · Confidence · Ease), match boldness to traffic. One hypothesis per test — not one element.
3. **BUILD** — pick the simplest test mode that expresses the hypothesis; set the goal as close to revenue as you can measure; QA the variation; exclude admins.
4. **PROVE** — real humans vote. Bayesian by default, bandit when losing traffic is expensive. Call a winner only above 90% confidence — ideally 95%. Never edit a live test.
5. **KEEP** — winner becomes the baseline. Original wins? That's a constraint learned, not a failure. Inconclusive? Valid result. Then back to SEE from the new baseline.

---

## What the agent actually does

The skill is opinionated about four things.

**When to test vs. ship.** Conversion pages, headlines, offers, prices, CTAs, forms, checkouts, funnel structure, and *any AI-generated conversion copy* get tested. Bug fixes, typos, legal text and accessibility fixes ship directly. When unsure: could this plausibly move conversion enough that knowing is worth the traffic?

**What to test first.** Offer → primary promise → CTA and conversion path → structure → trust → friction → imagery → micro-copy. Behavioural evidence overrides the list. ICE scores go in the test description so the owner can see the reasoning.

**How to measure.** A strict goal hierarchy: revenue / order value → completed conversion → qualified lead → purchase intent → engagement. *A CTA click is not a sale.* If the purchase can be tracked, track the purchase — including cross-domain when the checkout lives somewhere else. Sub-goals explain *why*: clicks up 40% with purchases down 8% means the page over-promised.

**How not to fool yourself.** Don't call a winner until AB Split Test's confidence is over 90% — ideally 95% — however far ahead one version looks today. Don't treat an emergency rollback as a result. Don't run tiny tests on low-traffic pages — only bold hypotheses reach confidence there. Don't re-run a disproved idea in a different colour.

The full skill is ~1,900 words and reads in five minutes: [`skills/ab-split-test/SKILL.md`](skills/ab-split-test/SKILL.md).

---

## Install

### Claude Code

```bash
git clone https://github.com/SiteSpot/human-web-feedback-loop.git
cp -r human-web-feedback-loop/skills/ab-split-test ~/.claude/skills/
```

Or project-scoped: copy into `.claude/skills/` in the repo you're working in.

### Codex / Cursor / other agents

Copy `skills/ab-split-test/SKILL.md` wherever your agent loads instructions from (`AGENTS.md`, `.cursor/rules/`, a system prompt, a skills directory). The file is plain Markdown with a small YAML front-matter block; nothing else is required.

### Connect the MCP server

The skill calls AB Split Test's MCP tools (`absplittest/create-test`, `get-test-results`, `get-heatmap-data`, …). Add the server to your agent's MCP config — setup for each client is in the [AB Split Test API / MCP / CLI docs](https://absplittest.com/api-mcp-cli/).

You need a WordPress site with [AB Split Test](https://absplittest.com) installed. The free tier is enough to try it; non-WordPress sites (Shopify, static, vibe-coded) can be tested through the plugin's split-test-server mode.

---

## Example

An agent is asked to "improve the booking page." With this skill loaded:

```
SEE    get-heatmap-data /book/ → 61% never reach the form; heavy clicks on the
       (non-linked) price text. get-page-analytics → exits cluster before the form.

FIND   Not a wording problem. Hypothesis: surfacing price framing + the booking
       CTA above the fold will lift completed bookings. ICE 8/8/9 → 8.3

BUILD  mode: magic. Primary goal: completed booking. Sub-goals: CTA click,
       form start. Audience: logged-out + customers, admins excluded.
       QA with ?abhash=1 on desktop + mobile. create-test → active.

PROVE  Day 9: bookings +22% at 96% confidence. Clicks +40%, form starts
       +31% — intermediate and final metrics agree, no over-promise.

KEEP   Variation becomes baseline. Learned: earlier price clarity beat more
       persuasive copy. Back to SEE — next bottleneck is probably the offer.
```

The owner gets: *"The price-led hero increased completed bookings ~22% with 96% confidence after 9 days."* Not a wall of statistics.

---

## What's in the repo

```
human-web-feedback-loop/
├── skills/
│   └── ab-split-test/
│       └── SKILL.md        ← the skill. This is the whole product.
├── README.md
├── CONTRIBUTING.md
└── LICENSE
```

One file does the work. Everything else is packaging.

---

## FAQ

**Does this only work with WordPress?**
The MCP server runs on a WordPress install, but that install can act as a split-test server for any site — add the domain, drop in a script, and the agent can test a Shopify store or a static page the same way.

**Will it slow the site / break caching / hurt SEO?**
The plugin handles cache purging (WP Rocket, LiteSpeed, Autoptimize, Cloudflare, Bunny), sets canonical tags on variations, and serves full-page tests server-side or at the Cloudflare edge for zero flicker. The skill tells the agent *not* to touch caching itself.

**What if the site doesn't have much traffic?**
The skill's rule: on low-traffic pages, test only bold differences. Two nearly identical variations will rarely reach 90% confidence on a small site — that's a test-design problem, not a traffic problem. Heatmaps and the CRO audit still work at any traffic level.

**Can the agent deploy winners on its own?**
For low-risk content tests (headline, CTA text, section order) yes, if auto-complete is enabled. For price, checkout, forms, payment logic or legally meaningful claims, the skill defaults to owner review.

**When does the agent call a winner?**
When AB Split Test reports over 90% confidence — ideally 95% — and the test has run at least a full week. Until then it keeps collecting, stops the test, or redesigns for a bolder difference. It never just picks whichever version is ahead.

---

## Contributing

Better heuristics, tighter wording, worked examples from real sites, ports to other testing tools — all welcome. See [CONTRIBUTING.md](CONTRIBUTING.md).

If you run this on a real site and it produces a decision, we'd love the anonymised story as an example.

---

## License

[MIT](LICENSE). Built by the people behind [AB Split Test](https://absplittest.com) — eight years of WordPress split testing, 2,400+ sites, every caching setup you can imagine.
