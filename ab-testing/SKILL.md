---
name: ab-testing
category: experimentation
description: Run trustworthy A/B tests — hypothesis format, variant design, sample size math, significance testing, guardrails, and stopping rules.
triggers:
  - "A/B test"
  - "split test"
  - "experiment design"
  - "sample size calculation"
  - "statistical significance"
  - "is this test result real"
  - "when to stop an experiment"
inputs:
  - hypothesis
  - baseline_metric
  - traffic_volume
  - minimum_detectable_effect
outputs:
  - test_design
  - sample_size_plan
  - analysis_result
  - decision_memo
related_skills:
  - experiment-prioritization
  - cro-audit
  - landing-page-optimization
  - signup-flow
  - funnel-analysis
  - marketing-paid/creative-testing
required_context:
  - .context/product-marketing.md
allowed_tools:
  - mcp:analytics
version: 1.0.0
---

## When to Use

Invoke when:
- Designing an experiment before writing any test code
- Someone wants to ship a change and needs evidence it works
- Declaring winners or losers from past test results
- Planning a testing program with velocity targets
- A test looks "done" but you suspect premature conclusions

## Workflow

### Step 1: Hypothesis Formation
Write the hypothesis in strict format:

```
If we [change X] for [audience Y],
then [primary metric Z] will [direction and amount]
because [mechanism M].
```

Quality checklist:
- Mechanism M is a real user-behavior reason, not a hope
- The hypothesis names ONE primary metric with a predicted direction
- The predicted effect size is stated (this drives sample size)
- Evidence backing exists — data, research, or at least a strong analogy

**Gate:** Hypothesis written in format, with mechanism, metric, and predicted effect size.

### Step 2: Variant Design
Design variants that isolate the variable:
- One change per variant — if two things differ, you can't attribute the effect
- Control stays exactly as-is; never test against a moving control
- Variant is production-quality — no broken layouts or placeholder copy
- Guardrail metrics defined now — revenue per visitor, conversion quality, churn proxies
- Audience and allocation decided (default 50/50 unless risk demands otherwise)

**Gate:** Control and variant defined, single-variable, with guardrail metrics listed.

### Step 3: Sample Size & Duration
Calculate before launching. For a conversion-rate metric (two equal groups, two-sided test):

```
n = (z_alpha/2 + z_beta)^2 * (p1*(1-p1) + p2*(1-p2)) / (p2 - p1)^2
```

- z_alpha/2 = 1.96 for 95% confidence; z_beta = 0.84 for 80% power
- p1 = baseline rate, p2 = expected test rate (p1 plus your predicted lift)
- n = conversions needed per variant; divide by daily conversion rate for duration

Shortcut for rough planning (continuous metrics, 80% power, 95% confidence):

```
n ~= 16 * sigma^2 / delta^2
```

where delta = minimum detectable effect and sigma = standard deviation.

Duration rules:
- Run at least one full business cycle (a week minimum, ideally two)
- Never stop early just because the result looks significant
- If the math says 8 weeks, shrink the expected effect or pick a higher-traffic page

These are standard statistical formulas, not rules of thumb. Use them.

**Gate:** Required sample size and duration calculated and written into the test plan.

### Step 4: Implementation & QA
Launch cleanly:
- Test tool configured; variant assignment verified on desktop and mobile
- No overlap with other tests on the same page or funnel
- Data check after 24-48h — traffic split as expected, events firing
- Sample ratio mismatch (SRM) check — variant counts within a few percent of allocation

**Gate:** Live traffic flowing to both variants at expected split; events verified.

### Step 5: Analysis
Read the results correctly:
- Frequentist view — p-value and confidence interval on the lift
- Significance is not the goal — the CI must exclude your minimum detectable effect in the predicted direction
- Practical significance first — a statistically significant +0.3% can be worthless
- Bayesian alternative — posterior probability the variant beats control (useful with priors; label the prior)
- Segment analysis is exploratory — treat segment wins as new hypotheses, not conclusions

**Gate:** Analysis states significance, effect size with CI, and practical meaning — without cherry-picking.

### Step 6: Guardrails & Stopping Rules
Decide stop rules before launch:
- Stop only on pre-registered criteria — harm to a guardrail metric, or a severe bug
- Never stop on early significance (peeking invalidates the math)
- If a guardrail metric drops beyond a pre-set threshold, pause and investigate
- Log any page changes mid-test — they can invalidate the experiment

**Gate:** Pre-registered stopping criteria written down and shared before launch.

### Step 7: Decision & Documentation
Close the loop:
- Decision framework — ship, iterate, or kill — based on CI vs MDE, not p-value alone
- Write the learning — what did we learn about the mechanism, win or lose?
- Update the experiment-prioritization backlog with follow-up ideas
- Archive the full analysis where the team can find it

**Gate:** Decision made with documented rationale; learnings recorded and backlog updated.

## Practitioner Grounding & Decision Rules

Built from Ronny Kohavi (Microsoft), Georgi Georgiev (abtestingstats), Martin Goodson (Bayesian, VWO), Lukas Vermeer (Booking.com). Full research: practitioner-intelligence/syntheses/cro.md.

- **Peeking nuance** (Kohavi — EMPIRICAL, T1): peeking at results is fine for *aborting harm* (guardrail breach, severe bug) or *not acting*; it is never fine for *stopping for a win* without a pre-planned sequential design. The 3-case rule: abort / don't act / win. Peeking with stopping-for-win inflates false positives by orders of magnitude (Georgiev; Heap case: >60% false-positive rate).
- **Significance threshold is a business calculation** (Georgiev — FRAMEWORK, T1): 95% is a default, not a law. Lower thresholds are justified when tests are cheap and opportunity cost is high; keep strict thresholds for irreversible/high-stakes changes. Encode cost/benefit inputs, not ritual.
- **Bayesian vs frequentist** (Goodson vs Georgiev/Kohavi — DISAGREEMENT, conditional): Bayesian monitoring (probability of being best + expected loss + optional stopping) is legitimate for decisions with a stated threshold of caring; fixed-horizon frequentist (or pre-planned sequential with spending functions) when the analysis must be auditable. Both schools reject unplanned stop-for-win.
- **Sample ratio mismatch check** (Kohavi, Vermeer — EMPIRICAL, T1): SRM at p≈1.8e-6 for a 50.2/49.8 split has been observed in production; every test needs an SRM check before analysis.
- **A/A tests** (Kohavi — EMPIRICAL, T1): validate tooling with A/A tests; the p-value distribution should be uniform.

Decision rules:
1. IF results look significant before the pre-planned analysis time THEN do NOT stop for the win — either continue to the planned end or switch to a pre-planned sequential design (Kohavi/Georgiev — EMPIRICAL, T1).
2. IF a guardrail metric breaches a pre-set threshold or a severe bug appears THEN abort immediately — this is the only unplanned stop that is allowed (Kohavi — EMPIRICAL, T1).
3. IF the analysis must survive external audit (regulatory, exec review, publication) THEN use fixed-horizon frequentist or pre-planned sequential — not ad-hoc Bayesian monitoring (Georgiev — FRAMEWORK, T1).
4. IF tests are cheap and reversible and opportunity cost is high THEN consider a lower significance threshold (e.g. 90%) computed from business cost/benefit (Georgiev — FRAMEWORK, T1).
5. IF sample size math says 8 weeks THEN do not shrink the effect to fit 2 weeks — pick a higher-traffic page or accept the duration (Kohavi n ∝ σ²/Δ² — EMPIRICAL, T1).
6. IF segment analysis shows a subgroup "win" THEN treat it as a new hypothesis, never a conclusion (Kohavi — EMPIRICAL, T1; existing guidance reinforced).
7. IF a test's variant counts diverge from allocation by more than a few percent THEN check SRM before reading results — the test may be invalid (Vermeer — EMPIRICAL, T1).

## Metrics

- **Pre-registered primary metric + guardrails** (OEC): the decision metric must be revenue-linked; guardrails catch quality losses (Kohavi — EMPIRICAL, T1).
- **Practical significance**: CI vs MDE, not p-value alone — a significant +0.3% is worthless (existing Step 5 — FRAMEWORK, T1).
- **Program diagnostic**: win rate 20-40% band (below = cosmetic/researchless; above = too safe) (synthesis — HEURISTIC, T2).
- **SRM and A/A p-value uniformity** as instrumentation health metrics (Kohavi/Vermeer — EMPIRICAL, T1).

## Sources

1. Ronny Kohavi et al., *Trustworthy Online Controlled Experiments* (book) + KDD keynote "Online Controlled Experiments: Lessons from Running A/B/n Tests for 12 Years" | exp-platform.com | tier 1 | 2026-08-14
2. Georgi Georgiev, *Statistical Methods in Online A/B Testing* | abtestingstats.com | tier 1 | 2026-08-14
3. Lukas Vermeer, experimentation methodology (Booking.com) | lucasvermeer.blogspot.com | tier 1 | 2026-08-14
4. Martin Goodson, *Smooth Bayesian A/B Testing* + VWO SmartStats documentation | VWO | tier 1 | 2026-08-14
5. Microsoft Research, optional stopping in Bayesian A/B testing | tier 1 | 2026-08-14

## Evaluation & QA

### Common Failure Modes
- Launching without sample size math, then "declaring" on noise
- Testing multiple changes at once and guessing which one mattered
- Peeking daily and stopping at the first significant reading
- Confusing statistical significance with practical significance
- Ignoring guardrails — conversion up, but revenue per visitor down
- Segment-hunting until some subgroup shows a "win"
