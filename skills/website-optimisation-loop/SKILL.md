---
name: website-optimisation-loop
description: Use when someone asks you to improve a website's conversions or work out why a page isn't working, in whatever words they use ("this page isn't converting", "why aren't people signing up", "nobody's buying", "my landing page sucks", "run CRO", "what should we test"), or shares a URL and asks how to get more sales, signups or bookings from it. Also use to analyse heatmaps or test results, or to run continuous or autonomous experiments that keep optimising a site over time. Runs the SEE → FIND → BUILD → PROVE → KEEP loop with real visitor behaviour as the evidence, explicit autonomy levels, and statistical validity checks. Works with AB Split Test (WordPress or Cloud) or any experimentation platform. For a one-off website edit that might affect conversions, use prove-website-changes instead.
---

# The website optimisation loop

You will have opinions about what should work. So will the owner. Opinions are inputs. Visitor behaviour is evidence.

**SEE → FIND → BUILD → PROVE → KEEP**, and KEEP feeds back into SEE. Every round starts from what the last one proved, so the site gets better by accumulating evidence rather than swapping one guess for another.

Load the references only when you need them:

| Reference | Load when |
|---|---|
| [methodology.md](references/methodology.md) | Running any step in detail: what to look for, how to score, goals, guardrails, learning records |
| [statistics.md](references/statistics.md) | Reading results, deciding whether a test is valid, choosing bandit vs fixed split, explaining a verdict |
| [wordpress.md](references/wordpress.md) | The site runs AB Split Test for WordPress |
| [cloud.md](references/cloud.md) | The site runs AB Split Test Cloud, or has no WordPress |
| [example-booking-page.md](references/example-booking-page.md) | You want to see a full round worked through |

The MCP tool descriptions of whichever platform is connected are the source of truth for parameters and behaviour. These references explain *when* and *why*.

---

## Before you start: agree the autonomy level

Ask the owner which level they want, record it, and work within it. **Never raise your own level**, and treat a standing permission as covering only what it says.

| Level | You may | Needs approval |
|---|---|---|
| **1 · Approve each test** (default) | Analyse, propose, build variations, create tests as drafts, report | Every launch, pause, winner and deployment |
| **2 · Standing permission by category** | Launch and complete tests in the categories the owner named (for example headlines, CTA wording, section order, social proof), within declared guardrails and traffic limits | Anything outside those categories; any guardrail breach |
| **3 · Continuous optimisation** | Run successive rounds, build the next test from a winner, deploy winners, within the boundaries the owner approved | Anything outside the boundaries |

**At every level**, price, payments, checkout logic, legal or regulated claims, and destructive changes need explicit permission for that specific change. Winner modes that deploy automatically (autocomplete, auto-optimise, "show the winner to everyone") are deployments: use them only where the level allows deploying. Confirm the site and page before any write, and keep the hypothesis, evidence and approval in the test description.

---

## SEE — read the evidence first

- List active and completed tests, and read past learnings, so you don't collide with a running test or re-run a disproved idea. A recorded pattern whose *Apply to* names this page is a ready-made candidate.
- Read behaviour: heatmaps, scroll depth, page analytics, session replays, form drop-off. Add support questions and customer objections if you have them.
- **No behaviour data yet** (new site, no heatmaps, analytics just installed): run the cold-start audit in methodology.md. Its findings are heuristics, so they score low on confidence. Fix outright breakage now, and get recording switched on so the next round has evidence.
- Note each candidate page's weekly visitors and conversions. That decides how bold a test must be.
- Let behaviour choose the lever. Nobody reaches the CTA: test position, not wording. Clicks on something unclickable: unmet intent. Many CTA clicks, few purchases: the problem is downstream.

## FIND — the hypothesis most worth the traffic

- Every candidate cites the observation it came from. A competitor doing it, or "it reads better", is an idea, not evidence.
- Score with ICE (impact, confidence, ease). Default priority when evidence is equal: offer, promise, CTA and path, structure, trust, friction, imagery, micro-copy.
- **One hypothesis per test, not one element.** Several connected changes expressing one claim are fine. Unrelated changes bundled together teach nothing.
- **Match boldness to traffic.** Low-traffic pages need bold differences or the test will never decide.
- Write it down: *Because [evidence], we believe [change] will [move primary goal] for [audience], because [mechanism].*

## BUILD — variation, goal, guardrails, plan

- **Primary goal as close to revenue as you can measure:** revenue or order value, then completed conversion, then qualified lead, then intent. A click is a sub-goal, never the primary goal when something deeper is measurable.
- **Sub-goals explain why.** Clicks up with purchases down means the variation over-promised.
- **Guardrails can veto a winner.** Declare them (order value, refunds, errors, speed, a key segment) with the action on a breach.
- **Pre-register:** goals, guardrails, minimum runtime in whole weeks, the decision rule (the platform's thresholds), and what you'll do if the variation wins, loses, or nothing is decided by a review date. Don't change these after seeing data.
- QA on desktop and mobile; confirm tracking fires; exclude admins. A broken variation is an invalid experiment, not a losing one.
- Create the test. Launch it only as your autonomy level allows.

## PROVE — let real visitors decide

- **Validity before results:** visitors recording, traffic split as configured, no mid-test campaign or site changes, no tracking breaks.
- **The platform decides whether a test has met its decision criteria.** Use its reported verdict, probability, expected loss and blocking reasons. Don't compute your own verdict from partial numbers, and never call a winner because one version is ahead.
- Also respect the pre-registered runtime: at least one full weekly cycle, even if the platform's thresholds are met sooner.
- A result that looks too good is a bug until checked.
- Don't edit a running test. If a guardrail breaches or something revenue-critical breaks, pause and roll back. That's risk control, not a result.

## KEEP — learn, then go round again

- **Variation wins:** it becomes the baseline; the next test starts from it.
- **Original wins:** record the constraint learned. That's a result, not a failure.
- **Inconclusive:** the most common outcome. Record it; next time go bolder, measure deeper, or pick a page with more traffic.
- Record every test the same way: hypothesis, evidence, change, result, sub-goals, guardrails, decision, what it showed, the reusable pattern and where else it might apply, next idea.
- Report in the owner's words: *"The price-led hero is very likely better for completed bookings (98% probability after three weeks); best estimate +22%, order value unchanged."*
- Back to SEE on the new baseline.

---

## Using a platform other than AB Split Test

The loop is platform-neutral. Map the steps onto your platform's MCP tools or API: list tests, create a test as draft, read results, change status. Use its own decision rule and reported numbers. Treat "AI-optimised", "smart traffic" or "dynamic allocation" modes as multi-armed bandits: good for earning during a campaign, not for learning what works (see statistics.md).
