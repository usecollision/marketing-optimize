---
name: benchmark-frameworks
category: analytics
description: Use benchmarks without fooling yourself — observed vs benchmark vs heuristic, when benchmarks mislead, sources by channel, normalization.
triggers:
  - "industry benchmark"
  - "is our conversion rate good"
  - "benchmark comparison"
  - "what's a good CTR"
  - "normalize benchmark"
  - "heuristic vs data"
inputs:
  - own_metrics
  - comparison_goal
  - stage_and_category
  - benchmark_sources
  - assumptions_in_play
outputs:
  - evidence_classification
  - normalized_comparison
  - benchmark_memo
  - goal_prior
related_skills:
  - metrics-framework
  - funnel-analysis
  - cro-audit
  - ab-testing
  - attribution-model-selection
  - mmm-incrementality
required_context:
  - .context/product-marketing.md
allowed_tools:
  - mcp:web-search
  - mcp:analytics
version: 1.0.0
---

## When to Use

Invoke when:
- Someone quotes "the industry average" to justify a goal, budget, or panic
- Setting targets and needing an outside prior
- A dashboard flags a metric as red without a comparison basis
- Deciding whether a channel is performing before you have enough own data
- Reviewing a deck where numbers mix observed data and assumptions without labels

## Workflow

### Step 1: Classify Every Number's Evidence Type
- Four classes in trust order — observed (your own instrumented data), industry benchmark (published aggregate), heuristic (practitioner rule of thumb), assumption (untested belief)
- Label every number in the analysis with its class — heuristics masquerading as observed data is how bad decisions get made
- For published benchmarks, capture source, methodology, sample, and date — a number without these is an assumption wearing a benchmark costume

**Gate:** Every number in the current analysis classified and labeled with provenance.

### Step 2: Decide Whether the Benchmark Applies
- Ask three questions — same stage? same category and price point? same channel and geography?
- Benchmarks mislead when business models differ (PLG vs sales-led), markets differ (B2B vs B2C), or aggregates mix winners and losers
- Use benchmarks for calibration and gap-spotting, not target-setting alone — your own trend line is a better target than anyone else's average
- Heuristic: treat a benchmark as a useful prior only when you are roughly within an order of magnitude of the benchmark population on the dimensions that matter (labeled heuristic)

**Gate:** Applicability verdict per benchmark — apply, normalize, or discard, with reasons.

### Step 3: Source Benchmarks by Channel
- Paid social and search — platform-published data plus independent studies; expect platform data to be optimistic, read the methodology first
- Email — ESP benchmarks (open and click by industry); treat open rates as directionally useful only since privacy changes degraded their precision (labeled assumption — verify against your own deliverability data)
- Conversion rates by industry and device — published CRO studies; the wide variance is the headline, not the median
- SaaS metrics — public company filings and benchmark reports for growth, retention, CAC payback; normalize for stage — seed and public are different species
- Nothing beats your own baseline — collect about two months of observed data before benchmarking anything new (labeled heuristic)

**Gate:** Benchmark source list per channel with methodology notes and bias flags.

### Step 4: Normalize Before Comparing
- Normalize for stage (early vs mature), category (transactional vs considered purchase), price point, geo, traffic source mix, and season
- Example pattern — a paid-CPC comparison against an aggregate that includes brand search flatters the aggregate; split brand vs non-brand (labeled example, not a statistic)
- When possible, re-baseline the benchmark against your own cohort — compare the same metric computed on your own segment
- State ranges, not points — benchmarks are distributions; "2-5% is common, median 3%" beats "3.2%"

**Gate:** Comparison uses normalized, ranged, same-cohort numbers — not raw point values.

### Step 5: Use as Prior, Not as Verdict
- Use benchmarks to set priors for experiments (delegate to ab-testing) and to sanity-check targets (delegate to metrics-framework)
- Gap analysis — a large gap between your number and a normalized benchmark is a hypothesis generator, not a crisis or a brag
- Never budget or kill a channel on benchmark data alone — your own incrementality evidence outranks any aggregate (delegate to mmm-incrementality where possible)
- Document the benchmark in the decision memo with its class and caveats, so the next reader does not promote it to fact

**Gate:** Benchmarks used as priors or hypotheses with documented caveats, never as sole verdicts.

### Step 6: Keep the Benchmark Library Honest
- Maintain a shared library — source, metric, value range, sample, date, class, last-used
- Re-verify annually; benchmarks decay as platforms change
- When a heuristic keeps being right, promote it — test it and graduate it to observed data or retire it

**Gate:** Benchmark library exists, is dated, and is reviewed on a schedule.

## Evaluation & QA

### Common Failure Modes
- "The average is X, we're at Y, therefore we're bad" — no normalization, no distribution, no stage match
- Platform-published benchmarks taken at face value
- A heuristic quoted as if it were law (the classic "industry standard is 3x ROAS")
- Comparing week-one numbers to someone's year-five numbers
- Open rates treated as precise after privacy changes changed what they mean
- Benchmarks used to kill a channel that only your own incrementality test could judge
- An assumption from three slides ago reappearing as a hard number two decks later
