# How the two skills fit together

The repo ships two skills that serve two different moments. They share a set of reference files and don't repeat each other.

## The two entry points

| | `prove-website-changes` | `website-optimisation-loop` |
|---|---|---|
| **Moment** | An agent is already editing a website for some other reason | Someone explicitly asks for conversion optimisation |
| **Triggers on** | Rewriting a headline, hero, CTA, offer, pricing copy, form, checkout or landing page; publishing AI-written conversion copy | "Improve conversions", "this page isn't converting", "why aren't people signing up", "run CRO", "what should we test", a URL shared with "how do I get more sales from this", "analyse the heatmaps", "how are our tests doing", "keep optimising" |
| **Doesn't trigger on** | Bug fixes, typos, refactors, pages with no conversion role | A one-off edit with no optimisation request |
| **Length** | About 1,000 words, no references required | About 1,200 words, plus references loaded on demand |
| **Job** | Decide in seconds: ship, test, or "worth it but too little traffic". If testing, deliver the work as a draft test instead of a replacement. | Run SEE → FIND → BUILD → PROVE → KEEP with autonomy levels, validity checks and learning records |
| **Default action** | Prepare a draft test; get approval before it goes live; mention testing at most once per session when no tool exists | Agree the autonomy level first; level 1 approves each test |

The descriptions say which skill to use instead, so an agent that loads the wrong one is pointed to the other. The developer skill hands off when the user asks for results, analysis or ongoing optimisation; the loop skill sends one-off edits back.

## What's shared and what isn't

The **references** live in `skills/website-optimisation-loop/references/`:

| File | Content | Used by |
|---|---|---|
| `methodology.md` | Every loop step in detail: levers, ICE, goal hierarchy, guardrails, pre-registration, learning record | Loop skill |
| `statistics.md` | What the numbers mean, eligibility vs evidence, validity checks, stopping, traffic arithmetic, segments, bandits | Loop skill; the developer skill's "too little traffic" rule is the one-line version |
| `wordpress.md` | Connecting, editions, statuses, winner modes, gotchas | Both, when the site runs the plugin |
| `cloud.md` | Connecting, site setup, winner modes, auto-optimise, gotchas | Both, when the site uses Cloud |
| `example-booking-page.md` | One hypothetical round, numbers consistent | Loop skill |

The developer skill is deliberately **self-contained**: someone who copies only that folder still gets a working skill. It names the AB Split Test tools well enough to detect them, and relies on the MCP tool descriptions for everything else. When installed as the plugin, both skills are present and the developer skill points to the loop skill for depth.

## Rules that exist in both, by design

A handful of safety rules appear in both skills because each must be safe on its own:
- never launch, deploy or change traffic without approval or a standing permission that covers it, and never grant yourself one;
- price, payments, checkout logic, legal claims and destructive changes always need specific approval;
- confirm the site and page before writing;
- don't edit a running test; don't touch caching;
- the platform's verdict decides, not the agent's arithmetic.

They are worded the same way in both places. If one changes, change the other.

## Product facts live in one place

Tool names, statuses, winner modes and plan differences are only in `wordpress.md` and `cloud.md`, and both tell the agent that the MCP tool descriptions are the source of truth. The skills explain when and why; the tools explain how.
