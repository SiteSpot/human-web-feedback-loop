# Statistics: reading results without fooling yourself

**The platform decides whether a test has met its decision criteria.** It has the full data, the configured thresholds and the model. Your job is to set the test up so its verdict means something, check the result is valid, respect the pre-registered plan, and explain the verdict honestly. Don't compute your own verdict from partial numbers.

---

## What the numbers mean

**Probability to be best** (AB Split Test calls it probability, likelihood of winning, or winner confidence). This is a Bayesian posterior: given the data and the model's assumptions, the probability that this variation's true conversion rate is the highest. "96%" means *very likely better*, not *96% confidence* in the frequentist sense.

**Frequentist confidence level / p-value.** Different idea: how surprising the data would be if there were no difference. A 95% confidence level and a 95% probability to be best are not interchangeable, and you shouldn't describe one as the other. If a platform reports p-values, use its language; if it reports posteriors, use that.

**Expected loss.** If you pick this variation and you're wrong, how much conversion you expect to give up on average. A high probability with a large expected loss means "probably better, but if not, costly". Platforms that report it typically require it to be under a threshold before calling a winner.

**Effect size and range.** The best estimate of the lift and a range around it. A 22% best estimate with a range from +1% to +43% is honest; "+22%" alone isn't. Effects that stop early tend to be overestimated.

---

## Eligibility versus evidence

Platforms set minimums before they'll consider a verdict: AB Split Test's defaults are at least 50 visits per variation and 25 conversions per variation (both configurable). These are **eligibility requirements**, not proof of enough evidence. A test that has just cleared them can still be far from a reliable answer; the probability and expected loss do that work.

---

## Validity checks, before reading results

1. **Is anything being recorded?** Zero or very low visits usually means caching, consent settings, a blocked script or a wrong page scope. Check the platform's settings; don't touch caching yourself.
2. **Does the split match the configuration?** A 50/50 test showing 600 vs 400 is a sample-ratio mismatch: something is dropping visitors from one arm (redirects, caching, a script error, bots). The result can't be trusted whatever it says. Investigate, fix, restart.
3. **Did anything change mid-test?** Campaigns, promotions, price changes, outages, new tracking. Note them; a big one can invalidate the test.
4. **Is the effect stable over time?** A big early lift that fades is often novelty. Look at the daily series if the platform provides it.
5. **Twyman's law.** A result that looks too good — a double-digit lift on a busy page, a probability that jumps to 99% in a day — is usually a tracking bug, a bot, a broken variation or a split problem. Check before you report it.

---

## Stopping

**Pre-register a minimum runtime** in whole weeks (at least one full weekly cycle, preferably two) and a review date. Then:
- **Don't stop early because the numbers look good.** Checking results repeatedly and stopping the first time they cross a threshold raises the chance of choosing a worse variation, under Bayesian and frequentist methods alike. Respect the platform's verdict *and* the minimum runtime.
- **Do stop early for harm or invalidity:** a guardrail breach, broken checkout, wrong prices, a split mismatch, or tracking failure. That's a safety decision, not a result.
- **Review, don't auto-extend.** If the review date arrives with no verdict, decide deliberately: keep going if the platform's projection says a decision is close, stop as inconclusive, or redesign bolder. Around four to six weeks is a sensible review point; beyond that, seasonality and cookie churn increasingly pollute the sample.

---

## Traffic arithmetic, roughly

To detect a relative lift *L* on a baseline conversion rate *p* with conventional power, each variation needs roughly

```
visitors per variation ≈ 16 × p(1−p) / (p × L)²
```

| Baseline | Lift to detect | Visitors per variation | Conversions per variation |
|---|---|---|---|
| 3% | 30% | ~5,700 | ~170 |
| 3% | 15% | ~23,000 | ~690 |
| 3% | 5% | ~210,000 | ~6,200 |
| 10% | 15% | ~6,400 | ~640 |

It's a frequentist rule of thumb (Evan Miller), but it gives the right order of magnitude for any method: small lifts need a lot of traffic. If a page can't reach the numbers for a plausible lift within about six weeks, the test needs a bolder change or a busier page. Use the platform's own projection (likely duration) once a test is running.

---

## Segments

- **Declare the segments that may affect a decision before launch** (for example mobile vs desktop if the change is layout-heavy).
- **Exploratory segment analysis afterwards is fine** and often where the next hypothesis comes from. But slicing results until something looks significant is a multiple-comparisons trap. A post-hoc segment result is a lead for the next test, not grounds to declare a winner.
- **Watch for a key segment losing while the average wins.** That's a guardrail question, decided deliberately.

---

## Bandits versus fixed splits

**Fixed split (default):** traffic stays even; the result tells you what works. Use it when you want to learn.

**Multi-armed bandit (Thompson sampling):** shifts traffic toward the likely winner as data arrives. Use it when earning matters more than learning: short promotions, time-limited campaigns, high-value checkouts where sending traffic to a worse option is expensive.

Bandits don't produce a clean, transferable answer. They also struggle when conversion rates drift by day of week or conversions arrive late, and a shifting allocation can make pooled results misleading. Never report a bandit's traffic share as a win. Platform modes called "AI-optimised", "smart traffic" or "dynamic allocation" are bandits.

---

## Overlapping tests

Running several tests at once on the same site is usually fine; real interactions are rare. Make tests mutually exclusive only when one variation would break or invalidate another (two tests rewriting the same element, or a price test alongside a checkout redesign).

---

## Sources

Evan Miller, *How Not To Run an A/B Test*; Chris Stucchio, *Bayesian A/B testing decision rule* and *Don't use bandit algorithms*; Lukas Vermeer, SRM FAQ; Kohavi, Tang and Xu, *Trustworthy Online Controlled Experiments*; Dmitriev et al., *A Dirty Dozen* (KDD 2017); Statsig and Eppo documentation on sequential testing and peeking. Fuller list in the repo's `docs/research-2026-10.md`.
