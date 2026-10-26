<div align="center">

# human-web-feedback-loop

**Don't just make website changes. Prove they're improvements.**

Agent skills that make A/B testing a normal part of how AI coding agents build websites. When your agent rewrites a headline, redesigns a landing page or reworks a checkout, it keeps the current version live and tests its version against it, instead of overwriting something that works with something it merely believes will work better.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Skills](https://img.shields.io/badge/type-agent%20skills-38d7d3)](skills/)
[![Works with](https://img.shields.io/badge/works%20with-Claude%20Code%20%7C%20Codex%20%7C%20Cursor%20%7C%20any%20MCP%20client-365187)](#install)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

[Install](#install) · [What changes](#what-changes-when-its-installed) · [The two skills](#the-two-skills) · [Testing tools](#testing-tools) · [FAQ](#faq)

</div>

---

## Why

AI agents now write the copy, build the pages and restructure the funnels. They're confident and fast, and they have no idea whether the result converts better or worse than what it replaced. Neither does anyone else, until real visitors have seen both.

The fix isn't a smarter model. It's a feedback loop: show some visitors the old version and some the new one, measure what they actually do, keep the winner. These skills put that loop into the agent's normal workflow, so testing happens because the agent changed something that matters, not because someone remembered to ask.

---

## Install

**Claude Code, as a plugin** (both skills):

```bash
/plugin marketplace add SiteSpot/human-web-feedback-loop
/plugin install human-web-feedback-loop@human-web-feedback-loop
```

**Any skills folder:**

```bash
git clone https://github.com/SiteSpot/human-web-feedback-loop.git
cp -r human-web-feedback-loop/skills/* ~/.claude/skills/
```

**Codex, Cursor and other agents:** copy `skills/prove-website-changes/SKILL.md` into wherever your agent reads instructions (`AGENTS.md`, `.cursor/rules/`, a system prompt). Add the optimisation-loop skill and its `references/` folder if you want the full CRO workflow.

Then connect a testing tool (see [below](#testing-tools)). Without one, the agent still tells you, once, when a change it made is untested.

---

## What changes when it's installed

You ask your agent to rewrite the homepage hero. Without the skill, it overwrites the hero. With it:

> The new hero is set up as a draft A/B test against the current one, judged on completed bookings. Activate it when you're ready, or tell me to just publish it.

That's it. The agent did the work you asked for, delivered it as a variation instead of a replacement, picked a goal closer to revenue than a click, and left the launch to you.

It stays out of the way the rest of the time:
- **Typos, bug fixes, accessibility, refactors** ship normally, with no mention of testing.
- **Pages with too little traffic** to give an answer get the change shipped, with one line saying it's unproven.
- **"Just publish it"** gets one sentence about the trade-off, then the agent publishes.

---

## The two skills

| | [`prove-website-changes`](skills/prove-website-changes/SKILL.md) | [`website-optimisation-loop`](skills/website-optimisation-loop/SKILL.md) |
|---|---|---|
| **For** | Agents already editing a site | Someone asking to improve conversions |
| **Triggers on** | Changing a headline, CTA, offer, pricing copy, form, checkout or landing page | "Improve conversions", "this page isn't converting", "what should we test", "analyse the heatmaps", "keep optimising" |
| **Does** | Ship, test, or flag as unproven. If testing, create a draft test instead of overwriting. | The full SEE → FIND → BUILD → PROVE → KEEP loop, with autonomy levels and validity checks |
| **Size** | About 1,000 words, self-contained | About 1,200 words, plus references loaded only when needed |

### The optimisation loop

```
        ┌──────────────────────────────────────────────┐
        ▼                                              │
      SEE ──────► FIND ──────► BUILD ──────► PROVE ──► KEEP
   heatmaps,    hypothesis    variation,     real       winner = baseline;
   analytics,   worth the     goal ≈ revenue, visitors,  loss = learned
   past tests   traffic       guardrails     validated  constraint
```

Real visitor behaviour picks what to test. One hypothesis per test, not one element. Completed conversions and revenue matter more than clicks. Sub-goals explain results, and guardrails can veto a winner. Losses and inconclusive results are recorded as learnings, so the agent accumulates knowledge instead of guessing again.

**Autonomy is explicit.** At level 1, the default, the owner approves each test. At level 2 the agent may launch tests in categories the owner named. At level 3 it runs successive rounds within approved boundaries. Price, payments, checkout logic and legal claims always need specific approval, and the agent can never raise its own level.

**The platform decides.** The agent sets tests up so the verdict means something, checks validity, and reports the platform's verdict in plain words. It doesn't call winners from its own arithmetic or because one version is ahead.

References, loaded on demand: [methodology](skills/website-optimisation-loop/references/methodology.md) · [statistics](skills/website-optimisation-loop/references/statistics.md) · [WordPress](skills/website-optimisation-loop/references/wordpress.md) · [Cloud](skills/website-optimisation-loop/references/cloud.md) · [worked example](skills/website-optimisation-loop/references/example-booking-page.md). How the two skills avoid overlapping: [docs/how-the-skills-fit.md](docs/how-the-skills-fit.md).

---

## Testing tools

The skills work with any experimentation platform that an agent can reach over MCP or an API. They're written against AB Split Test, which is built for exactly this workflow:

- **[AB Split Test for WordPress](https://absplittest.com).** Runs inside the WordPress install, with data in the site's own database and no traffic meter. The free Lite plugin on wordpress.org covers the core test tools over MCP. The paid plugin adds heatmaps for agents, revenue goals, audiences and automatic winner modes.
- **[AB Split Test Cloud](https://absplittest.com/cloud/).** For any other site: Next.js, Astro, Webflow, Framer, Shopify themes, static HTML. One script tag, and the whole product is available as MCP tools. There's no dashboard; the agent is the interface. Signups open from the waitlist, starting with 100,000 free page views.

Using Optimizely, Convert, Statsig, PostHog, GrowthBook, VWO or another platform? Connect its MCP server; the skills use whatever list, create, results and status tools are available.

---

## FAQ

**Will my agent start asking whether to A/B test every edit?**
No. Most edits ship normally. The agent only tests material changes to conversion-critical pages. When it does, it prepares the test as part of the work rather than asking first.

**Will it launch tests on its own?**
Not unless you give it standing permission for a category of change. By default it creates drafts and you activate them.

**What if my site has very little traffic?**
Small changes can't be judged on small traffic. The agent ships them and says they're unproven, or suggests a bolder change that a test could actually judge.

**Does it only work with WordPress?**
No. AB Split Test Cloud works on any site with a script tag, and the skills work with other platforms too.

**Why does it care about revenue more than clicks?**
A variation that gets more clicks and fewer purchases has over-promised. The agent measures the deepest conversion it can, and treats clicks as a clue about why.

---

## What's in the repo

```
.claude-plugin/                       plugin and marketplace manifests
skills/
  prove-website-changes/SKILL.md      the developer skill
  website-optimisation-loop/
    SKILL.md                          the optimisation skill
    references/                       methodology, statistics, WordPress, Cloud, example
docs/
  how-the-skills-fit.md               triggers, sharing, what's duplicated on purpose
  mcp-recommendations.md              product changes that would make the skills more reliable
  research-2026-10.md                 how the CRO platforms describe their loops, and sources
```

---

## Contributing

Better heuristics, tighter wording, real worked examples, notes for other platforms: all welcome. See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[MIT](LICENSE). Built by the people behind [AB Split Test](https://absplittest.com).
