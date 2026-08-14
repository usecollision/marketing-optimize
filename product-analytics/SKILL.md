---
name: product-analytics
category: analytics
description: Design product analytics — event taxonomy, activation and retention metrics, cohort analysis, feature adoption, Mixpanel/Amplitude/PostHog selection.
triggers:
  - "product analytics"
  - "Mixpanel"
  - "Amplitude"
  - "PostHog"
  - "activation metric"
  - "retention analysis"
  - "feature adoption"
inputs:
  - product_model
  - existing_events
  - activation_hypothesis
  - tooling_options
  - stage_and_team
outputs:
  - tool_recommendation
  - event_taxonomy
  - activation_metric
  - retention_framework
  - feature_adoption_plan
related_skills:
  - metrics-framework
  - analytics-setup
  - funnel-analysis
  - dashboard-design
  - signup-flow
  - ab-testing
required_context:
  - .context/product-marketing.md
allowed_tools:
  - mcp:analytics
version: 1.0.0
---

## When to Use

Invoke when:
- Choosing or migrating between Mixpanel, Amplitude, PostHog, or a warehouse-native stack
- Nobody can answer "what do users do in week one that predicts retention"
- Events exist but are inconsistently named, missing properties, or untrusted
- Feature launches ship without any adoption measurement
- Debating whether the marketing analytics stack can double as product analytics

## Workflow

### Step 1: Decide Product vs Marketing Analytics
- Marketing analytics (GA4 and similar) answers acquisition questions — sessions, channels, campaign performance
- Product analytics answers behavioral questions — activation, retention, feature usage, per-user journeys
- Use product analytics when you need logged-in user behavior, per-user funnels, cohorts, or in-app events at scale
- Keep both when both question sets exist — one taxonomy, two destinations (delegate setup to analytics-setup)
- Tool selection heuristic: PostHog for self-host or startup budgets, Mixpanel or Amplitude for mature product teams, warehouse-native (Snowplow plus dbt) when data engineering exists — evaluate against your team, not just the feature list (labeled heuristic)

**Gate:** Decision written — which stack answers which questions, and who owns each.

### Step 2: Define the North Star and Activation Metric
- North star = the one core value event the company optimizes (delegate framing to metrics-framework)
- Activation = the earliest point where a new user experiences core value — often "completed N core actions," not "logged in"
- Use data, not vibes — find the action whose completion in week one predicts week-8 retention (correlation analysis on historical cohorts)
- Label the metric, define the window ("activated = created a project within 7 days"), and freeze it for a quarter

**Gate:** North star and activation metric defined with data-backed thresholds and a time window.

### Step 3: Design the Event Taxonomy
- Name events as object-action ("invite_sent"), never generic ("clicked_button")
- Define a small property set per event; require the properties segmentation needs (plan, user_type, feature)
- Use one identification strategy — anonymous ID stitched to user ID at signup, never two competing IDs
- Track identity changes explicitly (merge, logout, account switch) — unhandled ID merges corrupt every downstream report

**Gate:** Taxonomy written — event names, required properties, identity plan — and shared with engineering.

### Step 4: Instrument the Core Funnel and Retention
- Funnel signup to activation to habit to retained, with per-step timestamps
- Retention definitions — N-day (calendar) for habit products, unbounded (per-user) for infrequent-use products — pick one and document it
- Cohort by acquisition week or signup source, not just by feature flag
- Ship the instrumentation behind the product; do not wait for the redesign

**Gate:** Core funnel and retention queries answerable for any cohort on demand.

### Step 5: Measure Feature Adoption
- For every feature launch — adoption (who used it, when), depth (how much), stickiness (do they return), impact (does usage correlate with retention)
- Use release-based cohorts by first-use date, not cumulative counts that hide churn
- Track announcement-to-trial and trial-to-habit separately — most feature failure is discovery, not quality (heuristic — verify per feature)
- Deprecation check — features below a stated adoption floor for N months get kill consideration, not promotion

**Gate:** Every launched feature has an adoption report; launch review scheduled.

### Step 6: Report and Iterate
- Ship a weekly readout — activation, retention, feature adoption, experiment results — delegate layout to dashboard-design
- Pair every metric drop with a cohort question, not a blame question
- Review the taxonomy quarterly — unused events cost instrumentation budget and confuse analysis
- Feed activation insights back into signup-flow and lifecycle email decisions

**Gate:** Weekly readout live; taxonomy review on the calendar; insights flowing to growth owners.

## Practitioner Grounding
- **John Cutler** — event taxonomy discipline (object_action names), vanity-metric test (context/intent/actionability), identity-merge corruption (FRAMEWORK, T1).
- **Gibson Biddle** — proxy metrics as "% of users who do ≥X by time Y"; threshold/cohort metrics over averages; NSM needs inputs (FRAMEWORK, T1).
- **Avinash Kaushik** — dashboards for decisions; KPI hierarchy with targets; outlier alerting >3σ; "slay the data-puking dragon" (HEURISTIC, T1).
- **Krista Seiden** — measurement setup discipline: custom events/dimensions registered, identity stitched, retention windows set explicitly (TACTIC, T1).

## Decision Rules
1. IF an event name is generic (clicked_button) THEN rename object_action (invite_sent) with a defined property set (Cutler, TACTIC, T2).
2. IF defining activation/NSM inputs THEN use threshold-cohort form: "% of [segment] doing ≥[threshold] by [time]" (Biddle, FRAMEWORK, T1).
3. IF a metric lacks context/intent/actionability THEN mark vanity and replace (Cutler, FRAMEWORK, T1).
4. IF reporting averages THEN add the distribution/cohort view alongside — averages hide churn (Biddle, EMPIRICAL, T1).
5. IF identity merges are unhandled THEN fix before trusting any per-user report — merge bugs corrupt every downstream number (Cutler/Seiden, EMPIRICAL, T2).
6. IF a KPI deviates >3σ from its own baseline THEN escalate with a hypothesis, not blame (Kaushik, HEURISTIC, T1).
7. IF a metric moves but no tactic changes THEN drop it from the dashboard (Cutler, HEURISTIC, T1).
8. IF picking a tool THEN match to team: PostHog for self-host/startup budgets, Mixpanel/Amplitude for mature teams, warehouse-native when data engineering exists (skill-consistent, HEURISTIC, T2).

## Metrics
- Primary: activation rate (frozen definition), retention (N-day or unbounded, documented), feature adoption/depth/stickiness/impact per launch.
- Guardrails: event-taxonomy drift (distinct event-name count), identity-merge bug rate, dashboard metrics without targets (should be zero).
- Timebox: weekly readout; quarterly taxonomy review; re-validate activation definition quarterly.
- Re-measure when: new product surface ships, ID strategy changes, or a launch alters the funnel.

## Practitioner Failure Modes
- Running product analytics on GA4's session model and discovering per-user funnels are impossible (Seiden).
- Activation defined as "logged in" because it was easy, not because it predicts retention (Cutler/Biddle).
- Event-name drift ("signup", "sign_up", "user_signup") until analysis is a grep exercise (Cutler).
- Cumulative feature dashboards hiding that adoption stopped weeks ago (Biddle cohorts).
- ID merge bugs silently duplicating users (Cutler/Seiden).
- North Star picked by the loudest team instead of the data (Cutler).
- Metrics that become targets get gamed (Cutler, Goodhart in practice).

## Sources
1. John Cutler — What Are Vanity Metrics and How to Stop Using Them | amplitude.com/blog/vanity-metrics | T1 | 2026-08-15
2. Gibson Biddle — How do you establish product metrics to evaluate success? | askgib.substack.com/p/how-do-you-establish-product-metrics | T1 | 2026-08-15
3. Avinash Kaushik — Five Strategies for Slaying the Data Puking Dragon | kaushik.net/avinash/slaying-data-puking-dragon-effective-dashboards | T1 | 2026-08-15
4. Krista Seiden — Ultimate Guide to Setting up a GA4 Property | kristaseiden.com | T1 | 2026-08-15
5. Synthesis: practitioner-intelligence/syntheses/analytics.md | T1 | 2026-08-15

## Evaluation & QA

### Common Failure Modes
- Running product analytics on GA4's session model and discovering per-user funnels are impossible
- Activation metric defined as "logged in" because it was easy, not because it predicts retention
- Event names drifting ("signup", "sign_up", "user_signup") until analysis is a grep exercise
- Feature dashboards showing cumulative users ever, hiding that adoption stopped weeks ago
- ID merge bugs silently duplicating users — every report wrong by an unknown factor
- North star picked by the loudest team instead of the data
