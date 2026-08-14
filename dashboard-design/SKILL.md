---
name: dashboard-design
category: analytics
description: Design dashboards that drive decisions — exec vs operator vs self-serve tiers, metric hierarchy, layout patterns, alerting, vanity-metric defense.
triggers:
  - "dashboard design"
  - "executive dashboard"
  - "operational dashboard"
  - "what should be on the dashboard"
  - "vanity metrics"
  - "alert thresholds"
inputs:
  - audience_and_roles
  - metric_hierarchy
  - data_sources
  - decision_cadence
  - existing_dashboards
outputs:
  - dashboard_spec
  - metric_selection
  - layout_plan
  - alerting_rules
related_skills:
  - metrics-framework
  - analytics-setup
  - funnel-analysis
  - product-analytics
  - attribution-model-selection
  - marketing-paid/performance-reporting
required_context:
  - .context/product-marketing.md
allowed_tools:
  - mcp:analytics
version: 1.0.0
---

## When to Use

Invoke when:
- Someone asks for "a dashboard" and nobody can name the decision it serves
- An existing dashboard is forty tiles nobody reads
- Executives ask for daily numbers that only matter weekly
- Alerts fire constantly or never, so teams ignore them
- Metrics look green while the business is in trouble — a vanity metric smell

## Workflow

### Step 1: Define Audience and Decisions First
- For every requested dashboard, name: who reads it, how often, what decision it informs, what action follows a red number
- No decision, no dashboard — a read-only status page is a report, not a dashboard
- Tier by audience — exec (direction, monthly), operator (execution, daily or weekly), self-serve (analyst exploration, ad hoc)
- Kill or archive dashboards that fail this test — assume a meaningful share of existing dashboards qualify for deletion until audited (labeled assumption)

**Gate:** Every dashboard has a named audience, cadence, decision, and action written down.

### Step 2: Select Metrics by Hierarchy, Not Popularity
- Work down from the goal — north star, then driver metrics, then diagnostic metrics (delegate to metrics-framework)
- Exec tier gets 5-8 outcome metrics max; operators get the levers they control; self-serve gets everything with documentation
- Prefer ratios over absolutes where scale distorts — conversion rate, revenue per visitor, payback period
- Include a guardrail or two — metrics that must not trade off (margin, churn, support load)

**Gate:** Metric list per tier written, each tied to a lever the reader controls.

### Step 3: Choose Layout Patterns
- Top row — outcome metrics with period-over-period deltas and targets
- Middle — trends over time with context (target lines, previous period, seasonality annotation)
- Bottom — breakdowns (by channel, segment, plan) for diagnosis
- One chart, one question; label axes and units; color means status, never decoration
- Exec view fits on one screen with no scrolling; operator view is dense but action-oriented

**Gate:** Layout wireframe per tier approved by the actual reader, not the builder.

### Step 4: Set Targets and Alerting Thresholds
- Derive targets from goals and benchmarks — delegate discipline to benchmark-frameworks; never set alerts without a baseline
- Alert on decision thresholds, not noise — a metric moving within normal variance should not page anyone
- Use variance-aware alerting — alert when outside the expected range for that season and day, not on raw threshold crossing
- Every alert has an owner and a runbook; alerts without runbooks get muted, not kept "just in case"
- Cap alert volume — more than a handful of alert types and expect alert fatigue (heuristic — observe your team's response rate)

**Gate:** Each alert has a threshold, owner, and runbook; total alert volume reviewed.

### Step 5: Defend Against Vanity Metrics
- Apply the action test — if this number doubled overnight, what would you do differently? No action, no tile
- Watch for follower counts, raw impressions, pageviews without conversion context, cumulative "ever" numbers, revenue without margin
- Show vanity metrics only when they feed a real metric (impressions into CTR into conversion)
- Re-audit quarterly — dashboards accumulate tiles like drawers accumulate junk

**Gate:** Every tile passes the action test; vanity metrics removed or demoted to context.

### Step 6: Document, Automate, Maintain
- Every dashboard has a one-paragraph data dictionary — where numbers come from, refresh cadence, known caveats
- Note data quality caveats on the dashboard itself — a metric with a 2-day lag must say so
- Schedule a quarterly review — usage stats, tile cleanup, decision fit
- Archive, never silently edit, dashboards others depend on

**Gate:** Data dictionary and caveats visible; quarterly review on the calendar.

## Practitioner Grounding & Decision Rules

Named grounding: Avinash Kaushik (KPIs with targets, outlier focus), John Cutler (context/intent/actionability), Gibson Biddle (thresholds over averages). Confidence: T1 = verified primary; T2 = well-known; T3 = caution.

- IF a dashboard metric has no pre-assigned target AND no decision attached THEN remove it — "focus only on KPIs, eliminate metrics" (Kaushik, HEURISTIC, T1).
- IF the dashboard shows more than ~6 KPIs per exec level THEN re-hierarchy: ~6 for CEO, ~6 for CMO, diagnostics live in analysis views, not dashboards (Kaushik, HEURISTIC, T1).
- IF KPI performance is within normal range THEN don't surface it; report only outliers (>3σ from the mean) with a hypothesis (Kaushik, HEURISTIC, T1).
- IF a metric lacks context/intent/actionability THEN flag as vanity and fix or drop (Cutler, FRAMEWORK, T1).
- IF the dashboard reports averages THEN add threshold/cohort views — "% of users who do ≥X by Y" (Biddle, FRAMEWORK, T1).
- IF targets look sandbagged THEN benchmark externally before accepting (Kaushik, T1).
- IF a metric moves but nobody changes tactics THEN remove it from the dashboard (Cutler, T1).

## Metrics

- Primary: % of dashboard KPIs with target + owner + decision; outlier alerts issued per period; dashboard usage (who opens, what they act on).
- Guardrails: vanity-metric count, average-vs-threshold divergence, sandbagged-target detection.
- Timebox: quarterly KPI review; remove/add metrics only at review.

## Sources

1. Avinash Kaushik — Five Strategies for Slaying the Data Puking Dragon | kaushik.net/avinash/slaying-data-puking-dragon-effective-dashboards | T1 | 2026-08-15
2. John Cutler — What Are Vanity Metrics and How to Stop Using Them | amplitude.com/blog/vanity-metrics | T1 | 2026-08-15
3. Gibson Biddle — How do you establish product metrics to evaluate success? | askgib.substack.com/p/how-do-you-establish-product-metrics | T1 | 2026-08-15

Full synthesis: practitioner-intelligence/syntheses/analytics.md

## Evaluation & QA

### Common Failure Modes
- Building the dashboard before asking what decision it serves
- One dashboard for everyone — execs drown in details, operators starved of levers
- Alerts set on raw thresholds that fire every Monday because of normal weekly seasonality
- Green metrics hiding a red business — conversion up while revenue per visitor falls
- Charts without axes, units, or comparison lines — pretty wallpaper
- Forty-tile dashboards that load slowly and get read never
- "We'll add a data dictionary later"
