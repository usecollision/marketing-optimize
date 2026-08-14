---
name: funnel-analysis
category: analytics
description: Stage-by-stage funnel diagnosis — drop-off analysis, conversion math, cohort views, and friction identification with benchmark context.
triggers:
  - "funnel analysis"
  - "where are users dropping off"
  - "conversion rate dropped"
  - "analyze the funnel"
  - "funnel drop-off"
  - "cohort analysis"
inputs:
  - funnel_stages
  - analytics_data
  - benchmarks
  - traffic_volume
outputs:
  - dropoff_report
  - conversion_math
  - cohort_views
  - friction_hypotheses
related_skills:
  - metrics-framework
  - cro-audit
  - analytics-setup
  - landing-page-optimization
  - signup-flow
  - experiment-prioritization
required_context:
  - .context/product-marketing.md
allowed_tools:
  - mcp:analytics
version: 1.0.0
---

## When to Use

Invoke when:
- Overall conversion dropped and you need to find where
- Launching optimization work and need a data baseline
- Deciding which funnel stage to fix first
- A/B tests keep shipping without moving the top-line number
- Leadership asks "where do we lose people?"

## Workflow

### Step 1: Funnel Definition & Data Pull
Define stages in terms the analytics actually track:
- Agree stage definitions (e.g. viewed pricing = `pricing_viewed` event)
- Pull volumes and conversion counts per stage for a recent stable period
- Exclude anomalous periods (site outage, launch spikes, sale events)
- Note sample sizes per stage — a 20% drop on 12 users is noise

**Gate:** Funnel table with per-stage volume and conversion counts for a clean period.

### Step 2: Conversion Math
Compute the numbers that matter:
- Step conversion — rate from each stage to the next
- Overall conversion — entry to final conversion
- Overall = product of step conversions (e.g. 0.40 × 0.50 × 0.30 = 0.06)
- Marginal impact — what a 10% relative lift at each stage does to overall

The biggest absolute drop-off is not always the best fix. A 10% lift on a 70%-converting stage moves overall more than a 10% lift on a 20%-converting stage.

**Gate:** Step rates, overall rate, and marginal impact ranked by upside for every stage.

### Step 3: Drop-off Diagnosis per Stage
For each leak, segment the drop-offs before theorizing:
- Traffic source — same drop across channels, or one broken campaign?
- Device — desktop vs mobile gap (often the biggest hidden leak)
- New vs returning — existing users convert differently
- Where exits go — bounce, back, to pricing, to support docs
- Error evidence — rage clicks, console errors, form validation failures

**Gate:** Every material drop-off has a segmented breakdown showing who drops and where they go.

### Step 4: Cohort Views
Add the time dimension:
- Acquisition cohorts — do users from week N convert better than week N+1?
- Time-to-convert distribution — what share convert within 1/7/30 days?
- Behavioral cohorts — activated users vs not, feature usage before upgrade
- Look for cohort cliffs — a specific week where conversion permanently shifted

Cohorts separate "the funnel is broken" from "the mix of users changed".

**Gate:** Cohort table produced; any cohort cliff identified and dated.

### Step 5: Friction Identification
Turn data into mechanisms:
- For each leak, hypothesize the friction using the cro-audit LIFT framework
- Distinguish intent mismatch (wrong audience) from friction (right audience, blocked)
- Check message match — do visitors arriving from a source get what was promised?
- Rate evidence strength — measured (hard data), observed (recordings), inferred (heuristic)

**Gate:** Each top leak has 1-3 friction hypotheses with evidence strength rated.

### Step 6: Benchmark Context
Contextualize rates honestly:
- Internal baselines first — your own trend and best period beat any external number
- External benchmarks only as directional context, labeled with source and date
- Benchmarks are ranges, not targets — treat published averages as heuristic
- Same-model comparisons only — a PLG self-serve funnel is not an enterprise sales funnel

**Gate:** Each stage rate annotated with internal baseline, trend direction, and any sourced external range.

### Step 7: Action Plan
Convert analysis into decisions:
- Pick 1-2 stages to attack based on volume × upside × evidence strength
- Hand friction hypotheses to landing-page-optimization or signup-flow
- Log test ideas into the experiment-prioritization backlog
- Set a re-measurement date and the metric that defines success

**Gate:** Prioritized action plan with owners, hypotheses, and success metrics.

## Evaluation & QA

### Common Failure Modes
- Analyzing a funnel with inconsistent stage definitions (rates are meaningless)
- Treating tiny-sample drops as real signals
- Confusing mix shift with funnel breakage (cohort views catch this)
- Fixing the stage with the biggest drop-off instead of the biggest upside
- Comparing against benchmarks from a different business model
- Reporting rates without volumes — a rate on 50 users proves nothing
