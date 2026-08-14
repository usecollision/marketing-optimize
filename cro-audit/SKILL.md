---
name: cro-audit
category: cro
description: Systematic conversion audit identifying friction points, drop-offs, and optimization opportunities across your funnel.
triggers:
  - "CRO audit"
  - "conversion audit"
  - "why aren't people converting"
  - "landing page not working"
  - "low conversion rate"
  - "funnel optimization"
inputs:
  - website_url
  - analytics_data
  - funnel_stages
  - current_conversion_rates
outputs:
  - audit_report
  - friction_map
  - optimization_priorities
  - test_hypotheses
related_skills:
  - ab-testing-framework
  - landing-page-optimization
  - marketing-messaging/landing-page-copy
  - marketing-optimize/funnel-analysis
required_context:
  - .context/product-marketing.md
allowed_tools:
  - mcp:analytics
  - mcp:heatmap
version: 1.0.0
---

## When to Use

Invoke when:
- Conversion rate is below benchmarks for your industry
- Spending on traffic but not getting proportional conversions
- Recently redesigned pages and need to validate
- Before scaling paid spend (fix leaks first)
- Quarterly optimization review

## Workflow

### Step 1: Funnel Mapping & Data Pull
Map the full conversion funnel with current data:

`
[Traffic Source] → [Landing Page] → [Signup/Lead] → [Activation] → [Conversion/Purchase]
     100%              X%              Y%              Z%              W%
`

For each step, note:
- Current conversion rate
- Volume (sessions, visitors)
- Drop-off rate to next step
- Benchmark for your industry/model

**Biggest leak = largest absolute drop-off (not just lowest %).** 

**Gate:** Full funnel mapped with real data and biggest leak identified.

### Step 2: Heuristic Page Review
Audit key pages using the LIFT framework:

**L - Value Proposition:** Is the value clear within 5 seconds?
**I - Relevance:** Does the page match visitor intent/expectations?
**F - Clarity:** Is the message and action unambiguous?
**T - Urgency:** Is there a reason to act now vs later?
**+ Friction:** What's making it harder than it needs to be?
**+ Anxiety:** What concerns might prevent action?

Score each page:
| Page | Value Prop | Relevance | Clarity | Urgency | Friction | Anxiety | Total |
|------|-----------|-----------|---------|---------|----------|---------|-------|

**Gate:** Top 5 pages scored with specific issues identified per dimension.

### Step 3: Friction Identification
Catalog all friction points:

**Visual friction:**
- [ ] Cluttered layout / too many competing elements
- [ ] Unclear visual hierarchy (what to look at first)
- [ ] CTA doesn't stand out (color, size, placement)
- [ ] Mobile experience issues (tap targets, overflow, speed)

**Copy friction:**
- [ ] Headline doesn't match ad/search intent
- [ ] Value prop unclear or buried
- [ ] Too much text / wall of words
- [ ] Jargon or unclear language
- [ ] Weak or generic CTA text

**Form friction:**
- [ ] Too many fields (every field costs conversions)
- [ ] Unnecessary required fields
- [ ] No progress indicator for multi-step
- [ ] Unclear error messages
- [ ] No social login option

**Trust friction:**
- [ ] No social proof above the fold
- [ ] Missing security indicators
- [ ] No refund/guarantee/risk reversal
- [ ] Unaddressed objections

**Technical friction:**
- [ ] Page speed >3s (every second = 7% conversion loss)
- [ ] Broken elements or errors
- [ ] Redirect chains from ads to page

**Gate:** All friction points cataloged with severity rating (H/M/L).

### Step 4: Prioritized Recommendations
Build the optimization roadmap:

| Priority | Issue | Page | Expected Impact | Effort | Test Hypothesis |
|----------|-------|------|----------------|--------|-----------------|
| P0 | [critical friction] | [page] | [X% lift est.] | Low | If we [change], then [metric] will [improve by X%] because [reason] |
| P1 | [major friction] | [page] | [X% lift est.] | Medium | |
| P2 | [minor friction] | [page] | [X% lift est.] | High | |

Prioritization formula: Impact × Confidence / Effort = Priority Score

**Quick wins (this week):** High impact, low effort fixes
**A/B tests (next 2 weeks):** Changes worth testing
**Strategic overhauls (next month):** Bigger redesigns

**Gate:** Top 10 recommendations with test hypotheses and priority scores.

### Step 5: Measurement Plan
Define how you'll measure success:

- Primary metric: [conversion rate from X to Y]
- Secondary metrics: [time on page, scroll depth, form starts]
- Minimum detectable effect: [what % lift is meaningful?]
- Required sample size: [calculate for statistical significance]
- Test duration: [minimum 2 weeks or X conversions]

**Gate:** Clear measurement plan that prevents premature decisions.

## Evaluation & QA

### CRO Audit Quality Check
| Criteria | Score 1 | Score 3 | Score 5 |
|----------|---------|---------|---------|
| Data depth | No analytics, opinion only | Some data referenced | Full funnel data with drop-off rates |
| Issue coverage | 1-2 obvious issues | 5-10 issues found | 15+ issues across all friction types |
| Prioritization | Flat list | Some ranking | ICE/PIE scored with test hypotheses |
| Actionability | Vague advice | General direction | Specific changes with mockup/wireframe direction |
| Business alignment | Random fixes | Addresses biggest page | Targets highest-revenue funnel step |

### Common Failure Modes
- Fixing low-traffic pages (optimize where the volume is)
- Opinion-based changes without data or hypotheses
- Testing too many things at once (isolate variables)
- Declaring winners too early (<95% statistical significance)
- Ignoring mobile (often 60%+ of traffic)
- Optimizing for the wrong metric (form fills vs revenue)