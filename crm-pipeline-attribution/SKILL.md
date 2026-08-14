---
name: crm-pipeline-attribution
category: attribution
description: Attribute revenue through the CRM pipeline — lead-to-revenue mapping, campaign source tracking, pipeline velocity, closed-won and handoff design.
triggers:
  - "pipeline attribution"
  - "lead to revenue"
  - "pipeline velocity"
  - "closed-won attribution"
  - "sales marketing handoff"
  - "campaign source tracking"
  - "CRM attribution"
inputs:
  - crm_stage_definitions
  - lead_source_data
  - pipeline_data
  - closed_deal_data
  - handoff_process
outputs:
  - stage_map
  - source_tracking_spec
  - velocity_report
  - closed_won_attribution
  - handoff_design
related_skills:
  - attribution-model-selection
  - utm-governance
  - analytics-setup
  - metrics-framework
  - workflow-builder
  - marketing-intelligence/account-intelligence
  - marketing-channels/lifecycle-sequences
required_context:
  - .context/product-marketing.md
allowed_tools:
  - mcp:analytics
version: 1.0.0
---

## When to Use

Invoke when:
- Marketing reports MQLs while sales reports revenue, and nobody can connect them
- Lead source data is a mess of "website", "event", and blanks
- Pipeline stages are fuzzy and deals stall without anyone noticing
- Debating which campaign gets credit for a closed deal
- Designing or repairing the marketing-to-sales handoff

## Workflow

### Step 1: Map the Lead-to-Revenue Journey
- Define every lifecycle stage with objective exit criteria — MQL, SQL, opportunity, proposal, closed-won, closed-lost (use the CRM's actual stage list, not an idealized one)
- Record timestamps for every stage entry — velocity math is impossible without them
- Map the data flow — web form, CRM record, source fields, opportunity, revenue
- Identify where records are created, deduplicated, and merged — attribution survives only if source fields survive merges

**Gate:** Stage map with exit criteria and timestamps written; data flow diagrammed end to end.

### Step 2: Capture Campaign Source at the Point of Entry
- Standardize UTM capture on every form (delegate to utm-governance) and store hidden source fields at first touch
- Preserve first-touch and last-touch separately — "most recent" fields overwrite history and destroy first-touch analysis
- Capture offline sources (events, referrals, sales-sourced) with a controlled vocabulary, not free text
- Enrich before scoring — firmographics, tech stack, intent signals (delegate to marketing-intelligence/account-intelligence)

**Gate:** Every new lead has first-touch, last-touch, and offline source stored and populated at a stated fill rate.

### Step 3: Measure Pipeline Velocity
- Velocity = (opportunities x win rate x average deal size) / cycle length — decompose it, never track only the aggregate
- Compute stage-to-stage conversion and median days in stage; flag deals aging past a threshold per stage
- Track leakage separately from slowness — a stage with high exit-to-lost is a different problem than a slow stage
- Cohort pipeline by source — the same total pipeline hides very different conversion profiles by channel

**Gate:** Velocity decomposed by component and by source; slowest and leakiest stages named.

### Step 4: Choose the Closed-Won Attribution Model
- Define the attribution unit — deal, revenue, or account — before touching any model
- First-touch for pipeline generation credit, last-touch for closing credit, multi-touch (U-shaped, W-shaped, custom weights) when both matter
- Delegate model selection discipline to attribution-model-selection; document assumptions and limitations in the same report as the numbers
- Reconcile against channel-level attribution — CRM attribution and paid-platform ROAS will disagree; explain the difference, never hide it
- Decide the rule for multi-campaign deals — a primary campaign field plus an influenced-by list beats pretending credit is clean

**Gate:** Attribution model chosen per question, documented, and reconciled with channel reporting.

### Step 5: Design the Sales-Marketing Handoff
- Agree the SLA — how fast sales follows up, what happens to uncontacted leads, when leads recycle back to nurture
- Define lead quality feedback — sales rates or rejects leads; marketing reads the reasons weekly (delegate automation to workflow-builder)
- Set a single definition of MQL and SQL owned by revenue leadership, not by whichever team is winning the argument
- Track handoff acceptance rate and time-to-first-touch as handoff health metrics

**Gate:** Handoff SLA, recycle rules, and quality-feedback loop written and agreed by both teams.

### Step 6: Report and Close the Loop
- Report pipeline by source, velocity by stage, and closed-won attribution monthly — delegate layout to dashboard-design
- Compare marketing-sourced vs sales-sourced pipeline quality, not just volume
- Run a quarterly lead-to-revenue reconciliation — total new revenue explained by source at the account level
- Feed learnings back — high-velocity sources get budget; slow sources get diagnosis before more spend

**Gate:** Monthly reporting live; quarterly reconciliation scheduled; source learnings feeding budget decisions.

## Evaluation & QA

### Common Failure Modes
- Source fields overwritten at every touch, so first-touch analysis is impossible
- Pipeline stages defined by vibes, so velocity math compares apples to oranges
- Attributing closed revenue to the last webinar email while ignoring the deal that was already in pipeline
- Handoff with no SLA — leads aging in the CRM while marketing reports "great MQL growth"
- CRM attribution and ad-platform ROAS shown in separate meetings and never reconciled
- Deals with no source treated as "unknown" and ignored, biasing every conclusion toward tracked channels
