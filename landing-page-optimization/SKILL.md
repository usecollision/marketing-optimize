---
name: landing-page-optimization
category: cro
description: Optimize landing pages systematically — message match, above-the-fold, form friction, trust elements, mobile, speed, and heatmap-informed hypotheses.
triggers:
  - "landing page optimization"
  - "improve landing page conversion"
  - "landing page not converting"
  - "message match"
  - "landing page audit"
  - "high bounce rate landing page"
inputs:
  - landing_page_url
  - traffic_sources
  - analytics_data
  - heatmap_data
outputs:
  - optimization_hypotheses
  - friction_inventory
  - test_plan
related_skills:
  - cro-audit
  - ab-testing
  - experiment-prioritization
  - analytics-setup
  - signup-flow
  - marketing-messaging/landing-page-copy
  - marketing-messaging/conversion-copywriting
  - marketing-messaging/objection-handling
required_context:
  - .context/product-marketing.md
allowed_tools:
  - mcp:analytics
  - mcp:heatmap
version: 1.0.0
---

## When to Use

Invoke when:
- A landing page has traffic but underperforms its conversion goal
- Launching a campaign and the page doesn't match the ad promise
- Heatmap or session data shows users dropping before the CTA
- Redesigning a page and want hypotheses instead of guesswork
- Need a prioritized list of tests for a page

## Workflow

### Step 1: Baseline & Goal
Anchor everything to data:
- Confirm the page's single goal (signup, demo, purchase)
- Pull current conversion rate, volume, and traffic-source mix
- Set the comparison baseline — same page last period, not a different page
- Note seasonality or campaign changes that pollute the baseline

**Gate:** Current conversion rate, volume, and source mix recorded with a clean baseline.

### Step 2: Message-Match Audit
Visitors arrive with a promise in their head:
- For each major source, compare the ad, email, or SERP result against the page headline
- Headline should repeat the source's promise within the first screen
- Message mismatch is the fastest fix and often the biggest leak
- Log concrete mismatches — ad says "free", page says "start trial"; SERP says "for designers", page says "for teams"

**Gate:** Source-to-page promise comparison table for every meaningful traffic source.

### Step 3: Above-the-Fold Review
The first screen decides most exits:
- 5-second test — can a stranger state what the page offers and what to do?
- Headline — concrete value, visitor's language, no internal jargon
- One primary CTA above the fold, visible without scrolling on common screens
- Visual hierarchy — what does the eye hit first, second, third?
- Remove or demote competing elements (auto-playing video, nav clutter)

**Gate:** Above-the-fold passes the 5-second test; single clear primary CTA identified.

### Step 4: Form & CTA Friction
Every field and step costs conversions:
- Cut fields to the minimum the business actually needs now
- Make optional fields visibly optional
- One CTA per intent; secondary actions visually subordinate
- CTA copy says what happens next ("Create free account", not "Submit")
- Error messages inline, specific, and recoverable

**Gate:** Field-level justification table — every remaining field has a stated business need.

### Step 5: Trust & Anxiety Reduction
Match trust elements to the specific objections:
- Social proof near decision points (customer logos, results, testimonials)
- Specificity beats volume — one named customer with a number beats ten generic quotes
- Risk reversal visible near the CTA (free trial, money-back, cancel anytime)
- Security cues only where they matter (payment, sensitive data)
- Address the top 3 objections from sales calls in the page copy

**Gate:** Each major objection mapped to a proof or risk-reversal element on the page.

### Step 6: Mobile & Speed
Fix the environment before the message:
- Mobile-first review — tap targets, overflow, sticky CTA, legibility
- Speed budget — measure LCP and INP, target Google's Core Web Vitals thresholds
- Name heavy assets — hero video, oversized images, third-party scripts
- Test on real mid-range mobile hardware, not just emulators

**Gate:** Mobile walkthrough logged with issues; speed metrics recorded with culprit assets named.

### Step 7: Heatmap-Informed Hypotheses
Let behavior data generate the tests:
- Scroll maps — where does attention die? Move critical content up
- Click maps — what gets clicked that isn't interactive? Make it a link or remove it
- Session recordings — watch a batch of sessions, log the moment intent breaks
- Rage clicks and dead clicks — indicators of broken or misleading UI
- Write each finding as a hypothesis in ab-testing format with a priority score

**Gate:** Every heatmap finding converted into a scored hypothesis, ranked by expected impact.

### Step 8: Prioritized Test Plan
Assemble the output:
- Rank hypotheses by impact estimate × confidence ÷ effort (see experiment-prioritization)
- Quick wins (copy, layout) separated from bigger tests
- No more than 1-2 tests per page concurrently to keep attribution clean
- Each hypothesis names the metric that must move to call it a win

**Gate:** Test plan with ranked hypotheses, success metrics, and sequencing.

## Practitioner Grounding
- **Michael Aagaard** — message match is the biggest copy lever: Saxo +99.4%, Bettingexpert +31.5% from headline/value-clarity changes (EMPIRICAL, T2).
- **Talia Wolf** — messaging audit and emotional targeting before layout work; analytics shows the leak, not why (FRAMEWORK, T2).
- **Peep Laja** — research before hypothesis; score ideas (PXL) before building; copy from customer research (FRAMEWORK, T2).
- **Jon MacDonald** — <1K visits/week: no valid small-effect A/B; rapid-test concepts or ship reversible changes (HEURISTIC, T2).
- **Georgi Georgiev** — cosmetic tests (button color, microcopy) are the field's #1 waste; changes must be noticeable (EMPIRICAL, T2).
- **MECLABS / Bryan Eisenberg** — LIFT model: value clarity, relevance, distraction removal (FRAMEWORK, T2).

## Decision Rules
1. IF ad/SERP promise ≠ page headline THEN fix message match before any other test (Aagaard, EMPIRICAL, T2).
2. IF page <1K visits/week THEN skip small-effect A/B; run one big-swing concept test or ship + honest before/after (MacDonald, HEURISTIC, T2).
3. IF a hypothesis is opinion-only THEN score it (Evidence/Impact/Effort/Traffic/ROI or PXL) and drop the bottom (Massey/Laja, FRAMEWORK, T2).
4. IF the proposed change is invisible to users (color, spacing, microcopy on low-traffic pages) THEN don't test it — spend the slot on message/value (Georgiev, EMPIRICAL, T2).
5. IF value prop is unclear or conversion leaks exist without explanation THEN run messaging research (customer language, Wolf's emotional-gap questions) before layout tests (Wolf, FRAMEWORK, T2).
6. IF mobile is >50% of traffic AND mobile conversion lags desktop THEN fix the mobile environment (speed, tap targets) before message tests (universal, HEURISTIC, T2).
7. IF redesigning a page THEN only after diagnosis — full redesigns reset learning and dip conversion for weeks (growwithba, HEURISTIC, T2).

## Metrics
- Primary: goal conversion rate by source (message-match compliance per source); revenue per visitor where trackable.
- Guardrails: bounce on matched vs unmatched sources; CTA click-through; form start vs completion; LCP/INP.
- Timebox: 2 full business cycles for any A/B decision; re-audit message match after every campaign change.
- Re-measure when: a new traffic source is added or an ad promise changes.

## Practitioner Failure Modes
- Testing cosmetics while message mismatch is the real leak (Aagaard, Georgiev).
- Redesigning before diagnosing (growwithba: redesigns dip conversion for weeks).
- Optimizing for one source while traffic comes from another (skill-consistent; Aagaard source-match).
- Heatmaps as verdicts — they generate hypotheses; tests decide (universal).
- Trust-element additions without testing (Aagaard privacy-policy case: −18.7%).
- Declaring winners from before/after without concurrent control (MECLABS).

## Sources
1. Michael Aagaard — copy/message-match case studies | aagaard.co / CXL | T2 | 2026-08-15
2. Talia Wolf — Emotional Targeting / messaging-first CRO | getuplift.co | T2 | 2026-08-15
3. Peep Laja — ResearchXL & PXL | conversionxl.com | T2 | 2026-08-15
4. Jon MacDonald — traffic thresholds (The Good) | thegood.com | T2 | 2026-08-15
5. Georgi Georgiev — "perfect shade of blue" / cosmetic-test waste | georgi.georgiev blog | T2 | 2026-08-15
6. MECLABS — LIFT model | meclabs.com | T2 | 2026-08-15
7. Synthesis: practitioner-intelligence/syntheses/cro.md | T1/T2 | 2026-08-15

## Evaluation & QA

### Common Failure Modes
- Redesigning before diagnosing (data first, opinions second)
- Testing cosmetic changes while message mismatch is the real leak
- Ignoring mobile when it is the majority of traffic
- Adding social proof without matching it to actual objections
- Optimizing the page for one source while paid traffic comes from another
- Declaring winners from heatmaps alone — heatmaps generate hypotheses, tests decide
