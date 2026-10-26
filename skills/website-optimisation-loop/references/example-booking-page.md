# Worked example: a booking page (hypothetical)

This is an illustration, not a real site. The numbers are invented but internally consistent.

**Site:** a booking page at `/book/` with about 4,000 visitors a week and a 3.0% booking rate (about 120 bookings a week). Autonomy level 1: the owner approves each test.

---

**SEE.** The heatmap shows 61% of visitors never scroll to the booking form, and repeated clicks on the price text, which isn't a link. Page analytics show exits clustering before the form. No test is running on the page, and past tests have nothing on price placement.

**FIND.** Not a wording problem: visitors want the price and a route to booking sooner.

> Because most visitors never reach the form and the price draws clicks, we believe moving the price and the booking button above the fold will lift completed bookings for all visitors, because it removes uncertainty and shortens the path.

ICE: impact 8, confidence 8 (behavioural evidence), ease 9, average 8.3. At 120 bookings a week, the test can detect a lift of around 30% in about three weeks (smaller lifts would take far longer), so this bold, structural change is testable; a button-colour tweak wouldn't be.

**BUILD.** Point-and-click test. One hypothesis, several connected elements: a price line and the booking button above the fold, everything else unchanged.

- Primary goal: completed booking (thank-you page).
- Sub-goals: booking button click, form start.
- Guardrails: average booking value, cancellations, mobile booking rate.
- Audience: all visitors except admins; device breakdown declared in advance.
- Plan: run at least two full weeks; decide on the platform's verdict (probability to be best of 95% or more, expected loss under threshold); review at four weeks. *Wins:* deploy, then test offer framing. *Loses:* record it, test the form next. *No decision at four weeks:* try a bolder version with the price in the headline.
- QA on desktop and mobile with the preview link; tracking checked. Created as a draft. The owner activates it.

**PROVE.**

| Day | Visitors per arm | Bookings (original vs new) | Platform says |
|---|---|---|---|
| 7 | ~2,000 | 60 vs 80 | Variation ahead, about 96% probability |
| 21 | ~6,000 | 180 vs 220 | Variation best, about 98% probability, expected loss below threshold |

On day 7 the variation looked like a winner, but the plan said two full weeks minimum and an early lead tends to overstate the effect, so nothing was decided. The split stayed at 50/50 and nothing else changed on the site. On day 21 the variation's booking rate is 3.67% against 3.00%: a best estimate of +22%, with a plausible range of roughly +1% to +44%. Button clicks rose 40% and form starts 31%. Booking value and cancellations were flat, and mobile moved with the average. The intermediate, final and guardrail metrics agree: the page didn't over-promise.

**KEEP.** The owner approves deploying the variation, which becomes the baseline. Learning record:

```
Hypothesis:  price and booking button above the fold → more completed bookings
Evidence:    61% never reach the form; clicks on non-linked price
Result:      variation best, ~98% probability, best estimate +22% (≈ +1% to +44%), 21 days
Sub-goals:   button clicks +40%, form starts +31%
Guardrails:  booking value, cancellations, mobile rate: unchanged
Decision:    deployed, owner approved
Learned:     earlier price clarity beat more persuasive copy; the lever was structure
Pattern:     visitors who hunt for the price convert more when it's above the fold
Apply to:    service pages that link to /book/; the pricing section of the homepage
Next:        test offer framing on the new baseline
```

Owner's report: *"Showing the price and the booking button above the fold is very likely better: 98% probability after three weeks. Best estimate is 22% more completed bookings, though the true effect could be anywhere from small to large. Booking value and cancellations didn't change."*

Back to SEE on the new page.
