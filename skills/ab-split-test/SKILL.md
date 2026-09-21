---
name: ab-split-test
description: How an agent should use AB Split Test while building, changing, or optimising a WordPress or connected website. Use whenever you create, rewrite, redesign, restructure, or materially change a page, section, offer, price, form, checkout, or conversion flow on a site with AB Split Test installed. Turns the agent from a designer replacing one guess with another into an optimisation system — observe behaviour, find the opportunity, build a hypothesis, prove it on real traffic, keep what works, learn from what doesn't, repeat.
---

# AB Split Test — the Proof Loop for agents

You will have opinions about what should work. So will the owner. Opinions are inputs. Visitor behaviour is evidence.

The operating loop is **SEE → FIND → BUILD → PROVE → KEEP** — and KEEP feeds straight back into SEE. Same five steps the product and its documentation use.

The rule in one line: **if a material change could plausibly help or hurt conversion, don't silently replace the existing version when you could learn from a test instead.** Testing isn't a final validation step. It's how you find out what to do next.

---

## 0 · Should this be tested at all?

Ask: *could this change plausibly alter conversion enough that knowing the answer is worth the traffic?*

**Test** — material changes to: homepage hero, landing pages, pricing, product pages, cart/checkout, signup/booking, lead forms · headline or primary promise · primary CTA wording/position/prominence · offer, price, guarantee, urgency · form length or checkout steps · section order or funnel structure · major social proof · **any AI-generated conversion copy** · large design changes · new checkout/booking/form/theme systems (use the test as a safe rollout with instant rollback).

**Ship directly** — bug fixes, broken links, typos, factual/legal/accessibility/security fixes, admin-only changes, pages with negligible traffic and no conversion role, changes the owner explicitly asked to publish untested, changes whose conversion impact is genuinely nil.

When unsure, weigh downside, upside, traffic available, page importance, and whether the result would teach you anything. A test that can't produce a decision wastes traffic.

---

## 1 · SEE — read the evidence before touching anything

**Check existing experiments first.** `absplittest/list-tests` — is a test already active on this page? Read completed tests so you don't re-run a disproved idea. Never overlap independent tests on the same page/audience unless designed to interact.

**Read behaviour.** `list-heatmap-pages` → `get-heatmap-data` (click, move, scroll) → `get-page-analytics` (entry, exit, journey). Look for: scroll depth, clicks on non-interactive elements, sections nobody reaches, drop-off before forms/pricing/checkout, gaps between what the page encourages and what visitors do.

**Let behaviour choose the lever:**
- Nobody reaches the CTA → test *position/structure*, not wording.
- People reach pricing then leave → offer, price framing, proof, or friction.
- Clicks on something unclickable → unmet intent; make it the thing they want.
- Many CTA clicks, few purchases → the problem is downstream of the CTA.
- Long form abandoned → test friction, not the hero.

---

## 2 · FIND — pick the hypothesis most worth the traffic

**Generate candidates** from heatmaps, analytics, funnel behaviour, previous tests, session replays, customer objections, support questions, and CRO heuristics.

**Default priority** (behavioural evidence overrides it): 1 offer · 2 primary promise/headline · 3 primary CTA and conversion path · 4 page/funnel structure · 5 trust and social proof · 6 form/checkout friction · 7 images/video · 8 micro-copy and cosmetics.

**Score with ICE (1–10 each, averaged):**
- *Impact* — if correct, how far does the primary business metric move? New offer on a busy checkout: high. Button radius: ~1.
- *Confidence* — how much evidence? Current-site behaviour, funnel data, repeated objections, prior tests: high. Heuristics, intuition, "sounds better": low. **A competitor doing it is an idea source, not confidence.**
- *Ease* — build time, breakage risk, QA, external dependencies.

Record the scores and reasoning in the test description. **Don't run a low-impact test because it's easy** — traffic spent proving something trivial can't prove something important.

**Match boldness to traffic.** High conversion volume: subtle tests are fine. Medium: one meaningful hypothesis. **Under ~100 conversions/month on the page: only bold hypotheses** — a different promise, offer, structure, or whole-page concept. Boldness means *expected difference in visitor behaviour*, not visual extremity. Two barely-different versions will never reach an answer; that's a design problem, not a traffic problem.

**One hypothesis per test — not one element.** A hypothesis like *"price and the booking action are hidden too far down; surfacing both above the fold will lift completed bookings"* may legitimately change the headline, a supporting line, price framing, the CTA, and section order together, because they all express one claim. What you must not do is bundle unrelated claims (new headline + lower price + testimonials + green button) — a win teaches you nothing about why. A full-page redesign can itself be the hypothesis.

**Write it down:**
> Because **[evidence]**, we believe **[change]** will **[affect primary goal]** for **[audience]**, because **[mechanism]**. Judged on **[primary goal]**; **[sub-goals]** used diagnostically.

Don't invent a precise expected uplift without a defensible basis.

---

## 3 · BUILD — the variation, the mode, the goal

### Mode
| Situation | Mode | Notes |
|---|---|---|
| Change one element's text/HTML/CSS/visibility on a live page | `magic` | Fastest. Selector-based. Verify with `?abhash=1`. |
| Compare whole pages or substantially different designs | `full_page` | Original vs a separate variant page. Canonical set automatically. Use Cloudflare edge when available for zero flicker. |
| Test builder sections, blocks, or template components | `ab_test` | Tag variations inside Elementor, Bricks, Breakdance, Oxygen, Divi, Beaver, Blocks. |
| Code-level CSS/JS variations | `css_test` | Only when the others can't express the hypothesis cleanly. |

Use the simplest mode that expresses the hypothesis safely.

### Primary goal — as close to revenue as you can measure
The goal is the most important decision in the test; the wrong goal produces a confident wrong answer. **Use the highest reliable level:**
1. **Revenue / order value** — WooCommerce, EDD, SureCart, FluentCart. Use *order value optimisation* if basket size could shift. Track paid/completed order states, never pending.
2. **Completed conversion** — purchase, completed booking, paid signup, submitted lead.
3. **Qualified lead** — form submission, tracked phone call (CallTrackingMetrics), **cross-domain conversion** on an external checkout (ThriveCart etc.). Set this up rather than settling for a click.
4. **Meaningful intent** — add-to-cart, start-checkout, pricing view, "book" click. Only when nothing deeper is trackable.
5. **Engagement** — scroll, time, secondary clicks. Sub-goal only, never primary.

A CTA click is not a sale. If the purchase can be measured, measure the purchase.

### Sub-goals explain *why*
Add CTA click, form start, checkout start, add-to-cart, scroll depth as sub-goals. Clicks +40% with purchases −8% means the variation over-promised or attracted worse-qualified intent — without the purchase goal you'd have declared a win.

### QA before launch
Keep everything unrelated identical. **Exclude admins/editors by role**; default audience is logged-out visitors, customers, subscribers. Exclude the owner's IP where practical. Load the variation with `?abhash=1`; confirm every selector resolves, desktop and mobile render, interactive states, forms and checkout work, tracking fires, and page speed holds if you added code. **A broken variation isn't a losing variation — it's an invalid experiment.**

### Statistics
Default **Bayesian, even split**. Use **multi-armed bandit (Thompson sampling)** when sending traffic to a worse option is expensive — promotions, high-value checkout, short campaigns — or when the owner values revenue preservation over a clean read. Fixed allocation when learning is the priority.

### Launch
`create-test` with mode, page(s), variation(s), primary goal, sub-goals, audience, hypothesis, ICE reasoning → `update-test-status` → `active`. Lifecycle: `idea → draft → queued → active → paused → complete → archived`. Create as `idea`/`draft` if the owner approves launches; otherwise activate after QA.

---

## 4 · PROVE — let real visitors decide, and don't interfere

Poll `get-test-results` daily; hourly for bandit tests on checkout or time-sensitive campaigns.

**Don't call a winner because** one version is ahead today, the result looks obvious, or anyone prefers the variation. Use the plugin's probability-to-beat / decision threshold. If it says not enough data, the options are: keep collecting, stop because it isn't worth more traffic, or redesign for a bolder difference. **Never "pick the one that's ahead." Never reinterpret noise as learning.**

Run at least one full weekly cycle even if the threshold arrives sooner. A threshold hit very fast should be sanity-checked against traffic quality, tracking, sample size, day-of-week, campaign changes, and technical issues.

**Don't edit an active variation.** If the hypothesis must change materially: stop, record what happened, create a new test.

**Damage control is a different decision from declaring a winner.** *Statistical decision:* do we have evidence to choose a lasting baseline? *Safety decision:* is there enough evidence of harm that continuing is irresponsible? If a variation looks like it's breaking a revenue-critical flow — checkout failures, purchase-rate collapse, wrong prices — verify tracking, `paused` (the plugin rolls back instantly), restore baseline, investigate. **An emergency pause is risk control, not proof the original won.** Don't turn a rollback into a false conclusion.

---

## 5 · KEEP — turn the result into learning, then go round again

**Variation wins:** it becomes the baseline. Record the result and the hypothesis, archive the test, and start the next test *from the winner*.

**Original wins:** don't say "the test failed." Record the constraint learned — *visitors want price before feature detail; removing reassurance cut bookings; stronger urgency raised clicks but not purchases.* A loss still reduces uncertainty. Don't re-run the same idea with cosmetic differences unless new evidence gives you a reason.

**Inconclusive:** a valid result. Record it; ask whether the hypothesis was bold enough, the goal correct, the traffic sufficient. Consider a bolder version. **Don't force a winner.**

**Auto-deploy:** fine for low-risk content tests — headline, CTA wording, section order, social proof — when the owner has enabled it. For price, checkout, forms, payment logic, custom JS, external systems, or legally meaningful claims, prefer owner review before permanent deployment unless explicitly authorised.

**Then back to SEE.** What changed? Where's the new friction? Did the win move the bottleneck elsewhere? What did the sub-goals reveal? Generate the next hypotheses from the new baseline. The site improves because you accumulate evidence instead of replacing one opinion with another.

---

## Guardrails

- Don't silently replace a material conversion-page change a test could have evaluated. If the owner insists, say once that it's untested, then comply.
- Don't create tests for trivial changes because testing is available.
- Don't overlap independent tests on one page/audience.
- Don't optimise for clicks when a deeper conversion is measurable; don't confuse engagement with business success.
- Don't call winners early; don't treat an emergency pause as a result; don't edit active tests.
- Don't run tiny tests on low-traffic pages.
- Don't touch caching yourself — the plugin purges WP Rocket, LiteSpeed, Autoptimize, Cloudflare, Bunny. If visitors aren't being recorded, check `get-settings` (cache/debug) before assuming the test is broken.
- Respect the plan's test limits (Lite: 1 site / 1 test / 1 variation).
- Record what was learned from wins, losses, and inconclusives.
- **Report in the owner's language:** *"The price-led hero increased completed bookings ~18% and crossed the decision threshold after 12 days"* — not posterior probabilities and observation counts. Add technical detail only when it helps.

---

## Tool reference

| Tool | Use |
|---|---|
| `absplittest/list-tests` | Active/completed tests; avoid collisions and repeats. |
| `absplittest/create-test` | Mode, pages, variations, goals, audience, hypothesis, settings. |
| `absplittest/get-test-details` | Inspect a test's configuration. |
| `absplittest/get-test-results` | Conversions, rates, probability-to-beat, time remaining. |
| `absplittest/update-test-status` | `active` / `paused` / `complete` / `archived`. |
| `absplittest/update-test-settings` | Allocation, goals, dates, statistics mode. |
| `absplittest/list-audiences` · `get-audience` · `create-audience` · `update-audience` · `delete-audience` | Role, referrer, device, URL-param, ad-platform, AI-assistant-source targeting. |
| `absplittest/list-heatmap-pages` · `get-heatmap-data` | Click, movement, scroll behaviour — the input to SEE. |
| `absplittest/get-page-analytics` | Entry, exit, journey. |
| `absplittest/get-settings` · `update-settings` | Cache/CDN, AI, privacy, edge worker, retention. |

---

## Worked example — booking page

**SEE.** `get-heatmap-data` on `/book/`: 61% never reach the booking form; repeated clicks on the price text, which isn't a link. `get-page-analytics` confirms heavy exits before the form. `list-tests`: nothing active on the page.

**FIND.** Not a wording problem — visitors want price clarity and a visible route to booking earlier. Hypothesis: *Because most visitors never reach the form and price draws strong interaction, surfacing price framing and the booking CTA above the fold will lift completed bookings by reducing uncertainty and shortening the path.* ICE: Impact 8, Confidence 8 (behavioural evidence), Ease 9 (`magic`) → 8.3.

**BUILD.** Mode `magic`. One hypothesis, several connected elements: price line above the fold, primary CTA beside it, rest unchanged. Primary goal: completed booking (form integration → thank-you page). Sub-goals: CTA click, form start. Audience: logged-out + customers; admins excluded. QA via `?abhash=1` on desktop and mobile; booking flow and tracking confirmed. `create-test` → `active`.

**PROVE.** Daily checks. After 9 days: completed bookings +22% at the configured threshold; CTA clicks +40%; form starts +31%. Intermediate and final metrics agree — the page didn't over-promise.

**KEEP.** Variation becomes the baseline. Archived with the learning: *earlier price clarity beat more persuasive copy.* Back to SEE on the new page — the next bottleneck is probably offer framing or checkout friction. Round two starts from what's proven.
