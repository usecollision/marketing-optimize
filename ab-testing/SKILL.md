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

## Evaluation & QA

### Common Failure Modes
- Launching without sample size math, then "declaring" on noise
- Testing multiple changes at once and guessing which one mattered
- Peeking daily and stopping at the first significant reading
- Confusing statistical significance with practical significance
- Ignoring guardrails — conversion up, but revenue per visitor down
- Segment-hunting until some subgroup shows a "win"
