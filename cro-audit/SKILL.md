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

## Practitioner Grounding & Decision Rules

Built from the CRO/experimentation canon: Peep Laja (CXL ResearchXL/PXL), Craig Sullivan (hypothesis format), Jon MacDonald (The Good), Talia Wolf (emotional CRO), Michael Aagaard (message-match), Brian Massey, Ben Labay (Speero), Ronny Kohavi (Microsoft), Lukas Vermeer (Booking.com), Georgi Georgiev (stats). Full research: practitioner-intelligence/syntheses/cro.md.

- **Research before hypothesis, hypothesis before test** (Laja, Sullivan, Kohavi, Labay — FRAMEWORK, T1): the ResearchXL method (6 research methods → 5 insight buckets) and the Sullivan hypothesis format ("We believe [A] for people [B] will cause [C]; we'll know via [D]/[E]") are the industry standard.
- **Instrumentation first** (Kohavi, Vermeer — EMPIRICAL, T1): validate tracking with A/A tests and SRM checks before trusting any audit data. SRM is common enough that "everyone who tests finds it" (Vermeer).
- **Message-match and value clarity are the biggest copy levers** (Aagaard — EMPIRICAL, T1): documented multi-site lifts from Get/My patterns (Saxo +99.4%, Bettingexpert +31.5%); Talia Wolf: run a messaging audit before touching elements.
- **Cosmetic tests are the field's #1 waste** (Georgiev, Kohavi, Wolf — EMPIRICAL/HEURISTIC, T1): button color and micro-copy tests on low-traffic pages can't reach significance; test strategies, not elements.
- **Traffic thresholds decide the method** (Kohavi, MacDonald, Laursen — FRAMEWORK, T1): below ~1K visits/week to a page, no valid small-effect test exists — rapid-test concepts, make reversible changes with honest before/after, or ship.

Decision rules:
1. IF tracking is unvalidated (no A/A pass, SRM detected) THEN fix instrumentation before auditing — results from broken tracking are noise (Kohavi/Vermeer — EMPIRICAL, T1).
2. IF the page gets <1K visits/week THEN do not design small-effect tests — use qualitative research, rapid concept tests, or reversible changes with honest before/after measurement (MacDonald/Laursen — FRAMEWORK, T1).
3. IF value clarity or message-match is broken (visitors don't get what you do) THEN fix messaging first — element-level CRO on a message-mismatched page is wasted effort (Aagaard/Wolf — EMPIRICAL, T1).
4. IF hypothesizing, THEN use the Sullivan format with a named mechanism — "We believe [A] for [B] will cause [C] because [mechanism]; we'll know via [D]" (Sullivan — FRAMEWORK, T1).
5. IF scoring opportunities THEN score against evidence provenance, not enthusiasm: measured data > user research > analogy > hunch (Laja PXL — FRAMEWORK, T1).
6. IF the audit's biggest leak is on a low-volume step THEN optimize where the volume is, not where the percentage is (cro-audit practice + Kohavi's n ∝ σ²/Δ² math — EMPIRICAL, T1).
7. IF the funnel's primary metric isn't revenue-linked THEN redefine it — optimize revenue per visitor (RPU), not conversion rate alone; CR up with AOV down is a loss (DRIP/Atticus Li — EMPIRICAL, T2).
8. IF the business is pre-PMF THEN CRO is premature — validate offer-market fit before conversion work (Collision cross-repo rule: research feeds CRO, not the reverse).

## Metrics

- **Primary**: revenue per visitor (RPU) at the audited step — not raw conversion rate (DRIP — EMPIRICAL, T2).
- **Diagnostic**: win rate 20-40% band for the testing program — below means cosmetic/researchless tests, above means too-safe tests (synthesis of NN/g, DRIP, growwithba data — HEURISTIC, T2).
- **Guardrails**: AOV, lead quality, downstream activation — a conversion gain that degrades revenue quality is a loss (Kohavi OEC — EMPIRICAL, T1).
- **Program**: decisions supported per quarter, implementation rate — not tests run (Vermeer — EMPIRICAL, T1).

## Sources

1. Ronny Kohavi et al., *Trustworthy Online Controlled Experiments* + "Online Controlled Experiments: Lessons from Running A/B/n Tests for 12 Years" (KDD) | exp-platform.com | tier 1 | 2026-08-14
2. Georgi Georgiev, *Statistical Methods in Online A/B Testing* | abtestingstats.com | tier 1 | 2026-08-14
3. Lukas Vermeer, experimentation talks (Booking.com) | lucasvermeer.blogspot.com | tier 1 | 2026-08-14
4. Peep Laja, ResearchXL + PXL frameworks | CXL blog | tier 1 | 2026-08-14
5. Craig Sullivan, hypothesis format + failure-mode talks | cxl.com / his decks | tier 1 | 2026-08-14
6. Michael Aagaard, message-match case studies (Saxo, Bettingexpert) | Unbounce/CXL | tier 1 | 2026-08-14
7. Talia Wolf, emotional CRO + messaging audits | GetUplift | tier 2 | 2026-08-14
8. Jon MacDonald, *Convert!* + The Good teardowns | thegood.com | tier 2 | 2026-08-14
9. Martin Goodson + Microsoft, optional stopping / Bayesian A/B testing | VWO/SmartStats, Microsoft paper | tier 1 | 2026-08-14

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