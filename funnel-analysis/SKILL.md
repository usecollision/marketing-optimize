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

## Practitioner Grounding
- **Peep Laja** (CXL) — ResearchXL: research (6 methods) before hypotheses; volume × upside × evidence prioritization (FRAMEWORK, T2).
- **Ronny Kohavi** — sample-size math: effect size, variance, traffic gate; tracking health (SRM/A-A) before any analysis (EMPIRICAL, T1).
- **Craig Sullivan** — hypothesis format + analytics health checks; never read rates without volumes and error bars (FRAMEWORK, T2).
- **Jon MacDonald** — below ~1K visits/week, small-effect A/B is invalid; use rapid concept tests or ship reversible changes (HEURISTIC, T2).
- **Gibson Biddle / John Cutler** — cohort and threshold views catch mix-shift masquerading as funnel breakage (EMPIRICAL, T1).
- **Talia Wolf** — analytics shows a leak, not why; message/emotional gaps need qualitative research (FRAMEWORK, T2).

## Decision Rules
1. IF tracking has SRM/known gaps THEN fix instrumentation before diagnosing any stage (Kohavi, EMPIRICAL, T1).
2. IF a stage drop is <1K visits/week THEN treat it as noise or hypothesis-only, never as a testable signal (MacDonald, HEURISTIC, T2).
3. IF choosing which stage to fix THEN rank by marginal impact (step rate × potential lift × volume), not by drop-off size (Laja, FRAMEWORK, T2).
4. IF a leak is confined to one source/device/segment THEN diagnose that segment — the rest of the funnel is not broken (Laja, TACTIC, T2).
5. IF cohort conversion shifted permanently at a date THEN hunt for product/market events before blaming pages (Biddle, TACTIC, T1).
6. IF using benchmarks THEN prefer internal baseline/trend; external ranges are labeled heuristics only (Massey/Kiss, HEURISTIC, T2).
7. IF a stage shows drop-off but data can't explain why THEN add qualitative evidence (recordings, surveys, message audit) before hypothesizing (Wolf/Laja, FRAMEWORK, T2).

## Metrics
- Primary: per-stage step conversion WITH volumes (a rate without volume proves nothing); overall conversion; marginal impact per stage.
- Guardrails: SRM status per test period; event-taxonomy drift; mix shift (source/device share).
- Timebox: re-measure 2 weeks after shipping any fix, then monthly; cohorts evaluated on a rolling 4-week cadence.
- Re-measure when: a campaign launch, site redesign, or product change could reset baselines.

## Practitioner Failure Modes
- Optimizing the biggest drop-off instead of the biggest upside (DRIP RPU lesson; Laja) — the field's classic error.
- Reporting rates without volumes — "a rate on 50 users proves nothing" (Kohavi, Sullivan).
- Confusing mix shift with funnel breakage — cohorts catch this (Biddle).
- Benchmark comparisons across business models (self-serve vs enterprise sales) (Kiss).
- Fixing the funnel while instrumentation silently breaks (Kohavi/Vermeer SRM).

## Sources
1. Peep Laja — ResearchXL / PXL frameworks | CXL blog | T2 | 2026-08-15
2. Ronny Kohavi et al. — Trustworthy Online Controlled Experiments / pitfalls papers | T1 | 2026-08-15
3. Craig Sullivan — hypothesis format + analytics health checks | T2 | 2026-08-15
4. Jon MacDonald — 1K-visits rule (The Good) | T2 | 2026-08-15
5. Gibson Biddle — proxy metrics / threshold cohort metrics | askgib.substack.com | T1 | 2026-08-15
6. John Cutler — vanity metric test | amplitude.com/blog/vanity-metrics | T1 | 2026-08-15
7. Synthesis: practitioner-intelligence/syntheses/cro.md + analytics.md | T1/T2 | 2026-08-15

## Evaluation & QA

### Common Failure Modes
- Analyzing a funnel with inconsistent stage definitions (rates are meaningless)
- Treating tiny-sample drops as real signals
- Confusing mix shift with funnel breakage (cohort views catch this)
- Fixing the stage with the biggest drop-off instead of the biggest upside
- Comparing against benchmarks from a different business model
- Reporting rates without volumes — a rate on 50 users proves nothing
