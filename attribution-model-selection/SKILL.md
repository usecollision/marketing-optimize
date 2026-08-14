---
name: attribution-model-selection
category: attribution
description: Choose and implement the right attribution model based on your stage, channels, and data maturity.
triggers:
  - "attribution"
  - "which channel is working"
  - "attribution model"
  - "how to measure channel contribution"
  - "incrementality"
  - "MMM"
inputs:
  - product_context
  - channels_active
  - data_maturity
  - budget_level
outputs:
  - model_recommendation
  - implementation_plan
  - measurement_framework
  - limitations_doc
related_skills:
  - tracking-setup
  - marketing-optimize/metrics-framework
  - marketing-paid/paid-strategy
required_context:
  - .context/product-marketing.md
allowed_tools:
  - mcp:analytics
version: 1.0.0
---

## When to Use

Invoke when:
- Spending on multiple channels and don't know what's working
- Need to decide where to allocate budget
- Leadership asking for channel ROI numbers
- CAC is rising and need to understand why
- Moving from single-channel to multi-channel

## Workflow

### Step 1: Assess Data Maturity
Understand what you can actually measure:

| Level | Data Available | Best Model | Typical Stage |
|-------|--------------|-----------|---------------|
| 1 | Basic analytics, no CRM | Last-touch (default) | Pre-seed, </mo spend |
| 2 | CRM + analytics connected | First-touch + Last-touch comparison | Seed, -20k/mo |
| 3 | Multi-touch data, UTMs, CRM events | Multi-touch (linear, position-based) | Series A, -100k/mo |
| 4 | Large datasets, statistical capability | MMM, incrementality tests | Growth, +/mo |

**Gate:** Data maturity level identified honestly (don't over-engineer).

### Step 2: Model Selection
Choose based on your situation:

**Last-Touch Attribution:**
- Best for: Direct response, single-channel dominance, early stage
- Pros: Simple, actionable, easy to implement
- Cons: Ignores awareness and consideration touches
- When to use: <3 channels, </mo spend

**First-Touch Attribution:**
- Best for: Understanding top-of-funnel, awareness investment
- Pros: Values discovery, helps justify brand spend
- Cons: Ignores what actually closed the deal
- When to use: Alongside last-touch for comparison

**Multi-Touch (Linear/Position-Based):**
- Best for: Multiple touchpoints, longer sales cycles
- Pros: More fair distribution, better budget decisions
- Cons: Requires complete tracking, still rule-based
- When to use: B2B SaaS, 3+ channels, + spend

**Data-Driven/MMM:**
- Best for: Large spend, many channels, statistical rigor
- Pros: Accounts for correlation vs causation, cross-channel effects
- Cons: Requires large data sets, statistical expertise
- When to use: +/mo spend, dedicated data team

**Incrementality Testing:**
- Best for: Validating specific channel contribution
- Pros: Causal (not just correlational), definitive answers
- Cons: Requires holdout groups, takes time, loses some revenue during test
- When to use: Any budget, to validate big channel decisions

**Gate:** Model selected with honest assessment of data/resource constraints.

### Step 3: Implementation Plan
For chosen model:

**Basic (Last/First Touch):**
- UTM parameters on all links (source, medium, campaign, content, term)
- GA4 configured with conversion events
- CRM captures first-touch and last-touch source
- Monthly comparison report

**Multi-Touch:**
- All Basic requirements plus:
- Touchpoint logging in CRM (every interaction timestamped)
- Cookie/identity resolution across devices
- Custom attribution model in analytics
- Weighted credit allocation rules defined

**Advanced (MMM/Incrementality):**
- All above plus:
- Spend data by channel by day/week
- External factors (seasonality, competitor activity, PR)
- Statistical model (regression-based or Bayesian)
- Geo-holdout or time-based incrementality tests designed

**Gate:** Implementation plan with specific tools, tracking requirements, and timeline.

### Step 4: Practical Framework (MER + Channel CAC)
Regardless of model, always track:

**Marketing Efficiency Ratio (MER):** Total Revenue / Total Marketing Spend
- Simple, holistic, hard to game
- Trend over time matters more than absolute number
- Rising MER = marketing getting more efficient

**Channel CAC:** Spend per Channel / Customers from Channel
- Even imperfect attribution gives directional signal
- Compare trends, not exact numbers
- Use for relative allocation, not absolute truth

**Blended CAC:** Total Sales + Marketing Spend / New Customers
- The number that matters most
- Must be < LTV/3 for healthy business
- Track monthly, set targets

**Gate:** MER + Channel CAC + Blended CAC calculated and tracked.

## Practitioner Grounding
- **Eric Seufert** — platform/last-click attribution overstates channel contribution; overlap is near-certain once >1 channel; MMM macro + campaign micro on separate cadences (EMPIRICAL, T2).
- **AdMaxxer / AdSights** — overlap tax: sum of platform ROAS × spend vs total revenue; >35% gap = platform ROAS fiction; MER target ≈ 1.3 / contribution margin; iROAS on brand search ~10–25% of reported (EMPIRICAL but vendor-sourced, T3).
- **Metricuno** — incrementality test design: ≥6–8 matched geo pairs, pre-test baseline, power checks — "spending €40k on a confounded holdout is worse than not testing" (EMPIRICAL, T2).
- **Binet & Field** — different metrics for different decisions: short-term with ROI, long-term with brand effects; don't measure brand with dollar estimates (FRAMEWORK, T1).
- **Avinash Kaushik** — every metric needs a target and a decision attached; metrics without decisions are decoration (HEURISTIC, T1).

## Decision Rules
1. IF <3 channels AND spend <~$20k/mo THEN last/first-touch comparison is enough — do not build multi-touch or MMM (maturity ladder, HEURISTIC, T2).
2. IF budget decisions rely on platform ROAS THEN compute MER and the overlap tax first; act on iROAS, not reported ROAS (Seufert/AdMaxxer, EMPIRICAL, T2).
3. IF suspecting a channel (brand search, retargeting, PMax) THEN run a valid incrementality test before cutting — holdout design with ≥6–8 pairs and baseline (AdSights/Metricuno, EMPIRICAL, T2).
4. IF evaluating MMM THEN require data volume (~$50–100k+/mo) and statistical capability; below that use MER + lift tests (Seufert, OPINION, T2).
5. IF a metric must move budget THEN attach it to a decision and a cadence (monthly drift / quarterly tests / annual split review) (Kaushik/Binet & Field, HEURISTIC, T1).
6. IF measuring brand THEN use brand-effect metrics (mental availability, share of search), not ROI gymnastics (Binet & Field, FRAMEWORK, T1).
7. IF MER drifts >10% in a month THEN investigate overlap/retargeting issues before reallocating (AdMaxxer, HEURISTIC, T3).

## Metrics
- Primary: MER at P&L level (target ≈ 1.3/contribution margin), blended CAC (< LTV/3), iROAS per channel for budget decisions.
- Guardrails: overlap tax (sum platform ROAS×spend / revenue − 1, watch >35%), platform ROAS used for creative only, incrementality-test sample validity.
- Secondary: MMM output (quarterly, at scale), share-of-search/branded volume for the brand stream, CAC payback window.
- Timebox: monthly MER review; quarterly incrementality tests on suspect channels; annual model re-selection vs stage.

## Practitioner Failure Modes
- Perfect-attribution chasing — model complexity beyond data maturity (maturity ladder).
- Over-crediting last touch and starving awareness work (Seufert; Binet & Field short-termism).
- Building MMM without data volume or capability (Seufert).
- Underfunded/confounded incrementality tests — worse than not testing (Metricuno, AdSights).
- Ignoring iOS privacy/cookie deprecation in model assumptions (Seufert).
- Measuring brand with ROI estimates → finance defunds it (Binet & Field, Ritson).
- Fixed model never revisited as the business changes (Francois).

## Sources
1. Eric Seufert — Mobile Dev Memo (attribution, incrementality) | mobiledevmemo.com | T2 | 2026-08-15
2. AdMaxxer — MER vs ROAS / overlap tax | admaxxer.com | T3 | 2026-08-15
3. AdSights — incrementality testing guides | adsights.io | T3 | 2026-08-15
4. Metricuno — incrementality test design | metricuno.com | T2 | 2026-08-15
5. Binet & Field — Long and the Short of It / Effectiveness in Context | IPA | T1 | 2026-08-15
6. Avinash Kaushik — dashboards/decision metrics | kaushik.net/avinash | T1 | 2026-08-15
7. Synthesis: practitioner-intelligence/syntheses/paid-strategy.md | T1/T2 | 2026-08-15

## Evaluation & QA

### Common Failure Modes
- Perfect attribution is a myth (accept directional accuracy)
- Over-crediting last touch (ignores all awareness work)
- Building complex models without sufficient data
- Not accounting for iOS privacy / cookie deprecation
- Attribution replacing judgment (it informs decisions, doesn't make them)
- Ignoring offline and word-of-mouth (self-reported attribution helps)