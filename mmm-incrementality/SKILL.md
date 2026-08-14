---
name: mmm-incrementality
category: attribution
description: Measure true marketing impact with geo experiments, holdout tests, and marketing mix modeling — incrementality vs attribution and practical design.
triggers:
  - "incrementality"
  - "geo experiment"
  - "marketing mix modeling"
  - "MMM"
  - "holdout test"
  - "is this channel actually working"
  - "platform ROAS vs real ROAS"
inputs:
  - channel_spend
  - revenue_data
  - geo_availability
  - measurement_questions
outputs:
  - measurement_design
  - experiment_plan
  - incrementality_findings
  - budget_recommendations
related_skills:
  - attribution-model-selection
  - metrics-framework
  - analytics-setup
  - marketing-paid/paid-strategy
  - marketing-paid/media-planning
  - marketing-paid/performance-reporting
required_context:
  - .context/product-marketing.md
allowed_tools:
  - mcp:analytics
version: 1.0.0
---

## When to Use

Invoke when:
- Platform-reported ROAS looks great but revenue doesn't move
- Deciding whether a channel is truly incremental or stealing credit
- Planning budget reallocation with more than last-click data
- Building a measurement program beyond attribution models
- Asked "what would happen if we stopped spending here?"

## Workflow

### Step 1: Frame the Question
Start with the decision, not the method:
- Incrementality questions — does channel X cause revenue we wouldn't otherwise get?
- Saturation questions — are we overspending a channel with diminishing returns?
- Budget questions — what is the optimal allocation across channels?
- Attribution answers "which touchpoint gets credit" — incrementality answers "did the touchpoint matter"
- Write the decision the measurement must inform, then pick the method

**Gate:** Decision question written in one sentence; method chosen to answer it, not the reverse.

### Step 2: Method Selection
Match method to question and data reality:

| Method | Answers | Needs |
|---|---|---|
| Geo experiment | Does channel X drive incremental sales? | Market-level spend control, 2+ matched markets per arm |
| Audience holdout | Does channel X convert users who'd convert anyway? | Audience-level targeting control |
| Conversion lift test | Does this campaign cause conversions? | Platform lift-test support |
| Ghost ads | Does channel X drive incremental conversions (auction-level)? | Ad-placement control at auction level (Facebook, Google) |
| MMM | What's the overall spend-response curve? | Years of weekly data, stable measurement |

Combine methods — MMM for the always-on picture, geo/holdout tests as point checks that calibrate it.

**Gate:** Method(s) selected with justification tied to the question and available data.

### Step 3: Geo Experiment Design
The workhorse of incrementality:
- Select matched market pairs on pre-period sales, size, and mix (matching is the practical standard here)
- Run a pre-period of 2-4 weeks to verify similarity (heuristic — longer if seasonality is present)
- Randomize which market of each pair gets the treatment (e.g. turn off or double the channel)
- Minimum duration 2-4 weeks per period (heuristic — depends on purchase cycle length)
- Effect = difference in post-period sales between treatment and control, scaled to the change in spend
- Watch for spillover — users crossing market boundaries or national campaigns leaking in

**Gate:** Pair selection, randomization, and duration documented before launch.

### Step 4: Holdout & Lift Test Design
Audience-level checks:
- Holdout — exclude a small share (5-10% heuristic) of an audience from a channel entirely
- Compare held-out vs exposed conversion over the same window
- Conversion lift — split the target audience at impression level within the platform
- Mind the sample math — small holdouts yield wide confidence intervals (see ab-testing formulas)
- Log what the holdout sees instead (competitor ads, organic) — it changes interpretation

**Gate:** Holdout/lift design specifies audience, size, duration, and success metric.

### Step 5: MMM Basics
Understand what MMM can and cannot do:
- Model form — sales = base (organic, brand) + sum of channel contributions, each with adstock (carryover decay) and saturation (diminishing returns)
- Inputs — weekly revenue, spend by channel, price, promotions, seasonality, competitor proxies
- Practical data minimums — 2-3 years of weekly data, more if the mix changes often (heuristic)
- Strengths — full market view, cross-channel effects, budget optimization
- Limits — weak on short bursts and new channels, requires stable measurement, outputs are estimates with error bands

**Gate:** MMM feasibility assessed against data history; expectations documented (error bands, not point forecasts).

### Step 6: Measurement Design & Cadence
Assemble a program, not a one-off:
- Always-on MMM refreshed quarterly (or monthly with a provider)
- One geo or holdout test per quarter on the channel with the biggest budget question
- Pre-register each test's decision rule — what result changes the budget?
- Compare platform-reported ROAS vs measured incremental ROAS and publish the gap
- Feed results into budget reallocation and media-planning

**Gate:** 12-month measurement calendar with named tests, owners, and pre-registered decision rules.

### Step 7: Reporting & Decision
Translate measurement into action:
- Report incremental sales and ROAS per channel alongside platform numbers
- Use error bands — a channel with wide uncertainty deserves smaller bets
- State reallocation recommendations with expected impact as a range
- Track whether past measurement actually changed budgets (if not, the program is theater)

**Gate:** Report links each finding to a budget decision with stated uncertainty.

## Practitioner Grounding & Decision Rules

Built from Eric Seufert (ad economics), AdMaxxer/AdSights/Metricuno (incrementality practice), and Garrett Johnson (ghost ads research). Full research: practitioner-intelligence/syntheses/paid-strategy.md.

- **Seufert (T1)**: multi-channel spend guarantees redundant spend — the question is never "is my attribution right" but "what is each channel's incremental contribution". Run macro (MMM/MER) monthly/quarterly and micro (tests) weekly as separate loops. Payback window by cash position.
- **Garrett Johnson (T1)**: ghost ads are the cleanest incrementality method — auction-level randomization removes geo-matching confounds.
- **AdSights/Metricuno (T2, vendor)**: iROAS divergence benchmarks — brand search incrementality factor ~0.10-0.25x reported, retargeting ~0.20-0.35x; brand search can report 10x+ but true iROAS 1.5-3x. Digital lift tests need 5-15k users per arm. Geo tests need 6-8 matched markets and a pre-test baseline.

Decision rules:
1. IF deciding where to run the next incrementality test THEN test brand search and retargeting first — the two most over-credited channels (AdSights — EMPIRICAL vendor, T2).
2. IF designing a geo test with <6-8 matched markets or no pre-period baseline THEN redesign or don't run — "spending €40k on a confounded holdout is worse than not testing" (Metricuno — HEURISTIC, T2).
3. IF designing a digital lift test THEN size for 5-15k users per arm minimum, else expect inconclusive CIs (AdSights — EMPIRICAL vendor, T2).
4. IF the overlap tax (sum of platform ROAS × spend ÷ total revenue − 1) exceeds ~35% THEN platform ROAS is materially inflated; measurement findings should drive budget changes (AdMaxxer — EMPIRICAL vendor, T2).
5. IF a creative A/B test claims "incrementality" THEN reject the framing — creative tests measure relative creative performance, not channel incrementality (AdSights — EMPIRICAL vendor, T2).
6. IF measurement findings have not changed a budget in two quarters THEN the program is theater — pre-register each test's decision rule before launch (existing Step 6 + Seufert — T1).

## Metrics

- **iROAS** (incremental revenue ÷ incremental spend): the allocation-grade number. Benchmarks (directional): brand search ~0.10-0.25x reported ROAS; retargeting ~0.20-0.35x (AdSights/Metricuno — T2).
- **Overlap tax** = sum(platform ROAS × platform spend) ÷ total revenue − 1; >35% = platform fiction (AdMaxxer — T2).
- **MER** = total revenue ÷ total ad spend; target ≈ 1.3 ÷ contribution margin; the always-on truth layer (AdMaxxer — T2).
- **Reported-vs-incremental gap**: publish per channel; the gap IS the finding.

## Sources

1. Eric Seufert, "Media mix models are the future of mobile advertising"; "The emerging marketing economist" | mobiledevmemo.com | tier 1 | 2026-08-14
2. Garrett Johnson (via Seufert), "What is advertising incrementality?" | mobiledevmemo.com podcast | tier 1 | 2026-08-14
3. AdMaxxer, "Blended MER vs ROAS: When Each Breaks" | admaxxer.com | tier 3 (vendor) | 2026-08-14
4. AdSights / Metricuno incrementality guides | vendor blogs | tier 3 | 2026-08-14

## Evaluation & QA

### Common Failure Modes
- Running one geo test and treating it as eternal truth (seasonality, novelty)
- Holdouts too small to detect anything, then concluding "no effect"
- Comparing incremental ROAS to platform ROAS without noting they measure different things
- MMM without enough history — the model fits noise
- Ignoring spillover and misreading geo results
- Measurement findings that never change budgets
