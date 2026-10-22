# Methodology: SEE → FIND → BUILD → PROVE → KEEP in detail

The SKILL.md gives the shape of each step. This file is the detail behind it.

---

## 0 · Should this be tested at all?

Ask: *could this change plausibly alter conversion enough that knowing the answer is worth the traffic?*

**Test:** material changes to the homepage hero, landing pages, pricing, product pages, cart and checkout, signup and booking, lead forms; the headline or primary promise; the primary CTA's wording, position or prominence; offer, price, guarantee, urgency; form length or checkout steps; section order or funnel structure; major social proof; any AI-generated conversion copy; large design changes; new checkout, booking, form or theme systems (a test doubles as a safe rollout with instant rollback).

**Ship directly:** bug fixes, broken links, typos, factual, legal, accessibility and security fixes, admin-only changes, pages with negligible traffic and no conversion role, changes the owner asked to publish untested, changes whose conversion impact is genuinely nil.

**Worth testing, impractical:** the change matters but the page has too few conversions to decide in a sensible time. Ship it, say it's unproven, or make the change bold enough to be testable. See statistics.md for the traffic arithmetic.

A test that can't produce a decision wastes traffic.

---

## 1 · SEE

**Existing experiments.** List active tests on the page and audience you're considering. Overlapping independent tests is usually fine; make tests mutually exclusive only when one variation would break or invalidate the other. Read completed tests and learnings so you don't re-run a disproved idea.

**Behaviour.** Click and movement heatmaps, scroll depth, page analytics (entry, exit, journey), session replays, form analytics. Look for:
- sections few visitors reach;
- clicks on non-interactive elements;
- drop-off before the form, pricing or checkout;
- rage or dead clicks;
- gaps between what the page encourages and what visitors actually do.

**Voice of the customer.** Support questions, sales objections, reviews, survey answers. They explain *why* behind the heatmap's *what*.

**Let behaviour choose the lever:**

| Observation | Likely lever |
|---|---|
| Nobody reaches the CTA | Position and structure, not wording |
| Visitors reach pricing then leave | Offer, price framing, proof, or friction |
| Clicks on something unclickable | Unmet intent: make it the thing they want |
| Many CTA clicks, few purchases | Downstream: checkout, form, or over-promise |
| Long form abandoned | Friction, not the hero |
| Mobile converts far below desktop | Mobile layout and speed before copy |

**Traffic.** Write down weekly visitors and conversions for each candidate page now.

---

## 2 · FIND

**Generate candidates** from behaviour, funnel data, previous tests, learnings, objections and CRO heuristics. Every candidate cites the observation behind it.

**Default priority** (behavioural evidence overrides it): offer · primary promise or headline · primary CTA and conversion path · page or funnel structure · trust and social proof · form or checkout friction · images and video · micro-copy and cosmetics.

**ICE, 1–10 each, averaged:**
- *Impact*: if correct, how far does the primary business metric move?
- *Confidence*: how much evidence? Current-site behaviour, funnel data, repeated objections and prior tests score high. Heuristics and intuition score low. A competitor doing it is an idea source, not confidence.
- *Ease*: build time, breakage risk, QA, dependencies.

Record the scores and reasoning in the test description. Don't run a low-impact test because it's easy: traffic spent proving something trivial can't prove something important.

**Boldness and traffic.** Most real lifts are small, and most tests don't produce a winner. High-volume pages can test subtle changes. Low-volume pages can only test bold ones: a different promise, offer, structure or whole-page concept. Boldness means expected difference in visitor behaviour, not visual extremity.

**One hypothesis per test, not one element.** *"Price and the booking action are hidden too far down; surfacing both above the fold will lift completed bookings"* can legitimately change the headline, a supporting line, price framing, the CTA and section order together, because they all express one claim. Bundling unrelated claims (new headline, lower price, testimonials, green button) means a win teaches nothing about why. A full-page redesign can itself be the hypothesis.

**Write it down:**
> Because **[evidence]**, we believe **[change]** will **[affect primary goal]** for **[audience]**, because **[mechanism]**. Judged on **[primary goal]**; **[sub-goals]** used diagnostically; **[guardrails]** must not degrade.

Don't invent a precise expected uplift. If you pre-screen ideas with a model or "synthetic visitors", treat it as triage: published work finds models get the direction right more often than not but overstate the size.

---

## 3 · BUILD

### Primary goal hierarchy
Use the highest level you can measure reliably:
1. **Revenue or order value.** Count paid or completed orders, never pending. Judge on revenue per visitor if basket size could shift.
2. **Completed conversion.** Purchase, completed booking, paid signup, submitted lead.
3. **Qualified lead.** Form submission, tracked call, conversion on an external checkout. Set this up rather than settling for a click.
4. **Meaningful intent.** Add to cart, start checkout, pricing view, "book" click. Only when nothing deeper is trackable.
5. **Engagement.** Scroll, time, secondary clicks. Sub-goal only.

### Sub-goals
CTA click, form start, checkout start, add to cart, scroll depth. They explain the primary result. Clicks +40% with purchases −8% means the variation over-promised or attracted worse-qualified intent.

### Guardrails
Pass/fail metrics, declared before launch, each with an action:

| Guardrail | On a clear breach |
|---|---|
| Revenue per visitor, order value | Pause, roll back, investigate |
| Refunds, cancellations, support contacts | Investigate before any deploy |
| JS errors, checkout failures, page speed | Pause immediately |
| A key segment (mobile, paid traffic) falling while the average rises | Don't deploy without a segment-specific decision |

A variation can win its primary goal and still be unshippable.

**Holdouts.** A permanent holdout (a slice of visitors who always see the old baseline) measures the cumulative effect of a programme instead of summing individual lifts. It costs traffic, so use one only when traffic is high and the programme runs long enough to need the answer.

### Pre-registration
In the test description: hypothesis, ICE, primary goal, sub-goals, guardrails, audience, minimum runtime, review date, and the three outcomes with their actions (*variation wins → …; original wins → …; no decision by [date] → …*). Don't change any of it after seeing data.

### Mode
Use the simplest mode that expresses the hypothesis safely: an element-level change for copy, visibility and small layout; a separate variant page for whole-page concepts; builder or markup tagging for component tests; code-level CSS or JS only when nothing else can express it.

### Audience
Real visitors. Exclude admins, editors and the owner's IP where possible. Declare any segments you'll break results down by (device, traffic source, new vs returning) now, not after the results come in.

### QA
Preview every variation on desktop and mobile. Confirm selectors resolve, interactive states, forms and checkout work, every goal fires, there's no flash of the original, and page speed holds if you added code.

---

## 4 · PROVE

See statistics.md for the detail. In short:
- Check results on a schedule, daily for most tests. Checking is fine; acting on every fluctuation isn't.
- Validity before results.
- The platform's verdict decides whether the decision criteria are met; the pre-registered runtime must also have passed.
- Don't edit a running variation. If the hypothesis must change, stop, record, and start a new test.
- **Two different decisions.** *Statistical:* is there evidence to choose a lasting baseline? *Safety:* is there evidence of harm that makes continuing irresponsible? A guardrail breach or broken revenue flow calls for a pause and rollback. An emergency pause is risk control, not proof the original won.

---

## 5 · KEEP

**Variation wins:** it becomes the baseline. Separate the part of the hypothesis that was right from the parts that came along for the ride. Start the next test from the winner.

**Original wins:** record the constraint learned (*visitors want price before feature detail; removing reassurance cut bookings; stronger urgency raised clicks but not purchases*). Don't re-run it with cosmetic differences unless new evidence gives you a reason.

**Inconclusive:** valid and common. Ask whether the hypothesis was bold enough, the goal right, the traffic enough. Don't force a winner.

**Learning record**, the same for every test:

```
Test:        name, page, dates, runtime
Hypothesis:  because … we believed … would … because …
Evidence:    what it came from
Change:      what differed between versions
Result:      verdict, probability, best estimate and range of the effect
Sub-goals:   what moved and what didn't
Guardrails:  status
Segments:    declared segments that disagreed with the overall result (exploratory)
Decision:    shipped / kept original / no decision, and who approved
Learned:     the principle, in one sentence
Next:        the test this suggests
```

**Deploying winners.** Only as the autonomy level allows. Low-risk content can be deployed automatically where the owner has granted it. Price, checkout, forms, payment logic, custom code, external systems and legal claims need owner approval for the specific deployment.

**Report in the owner's language:** *"The price-led hero is very likely better for completed bookings: 98% probability after three weeks, best estimate +22%. Booking value and cancellations didn't change."* Add technical detail only when it helps.

**Then back to SEE.** Where's the new friction? Did the win move the bottleneck? What did the sub-goals show?
