---
name: crm-lead-ops
category: crm
description: Design lead operations — routing rules, lead scoring, lifecycle stages, MQL and SQL criteria, CRM data hygiene, sales-marketing SLA, and lead-to-revenue process.
triggers:
  - "lead routing"
  - "lead scoring"
  - "MQL criteria"
  - "SQL criteria"
  - "lifecycle stages"
  - "CRM data hygiene"
  - "sales marketing SLA"
  - "lead to revenue process"
inputs:
  - lead_sources
  - crm_stage_definitions
  - scoring_model
  - sales_capacity
  - slo_targets
outputs:
  - routing_rules
  - scoring_model
  - lifecycle_definitions
  - mql_sql_criteria
  - hygiene_policy
  - sla_design
related_skills:
  - crm-pipeline-attribution
  - metrics-framework
  - workflow-builder
  - attribution-model-selection
  - utm-governance
  - marketing-intelligence/account-intelligence
  - marketing-channels/lifecycle-sequences
  - marketing-channels/lead-sourcing-enrichment
required_context:
  - .context/product-marketing.md
allowed_tools: none
version: 1.0.0
---

## When to Use

Invoke when:
- Leads sit in the CRM unassigned, and sales follows up days or weeks late
- "MQL" means something different to marketing, sales, and the VP of revenue
- Scoring is a black box — points are handed out but nobody can explain why a lead is a 90
- The CRM is full of duplicates, blank source fields, and people who left their company years ago
- Marketing and sales argue over lead quality with no shared definition or feedback loop
- You're designing (or repairing) the process that moves a lead from first touch to closed revenue

## Workflow

### Step 1: Define Lifecycle Stages with Exit Criteria
Make every stage objective and timestamped:
- Enumerate the actual stages in your CRM — not an idealized list — from new lead through MQL, SQL, opportunity, customer
- Write one exit criterion per stage — what objectively moves a record forward (a behavior, a score threshold, a sales-qualified flag), not a vibe
- Record timestamps on every stage entry; without them, conversion-rate and velocity math is impossible
- Decide ownership per transition — which team or system is responsible for moving a record from one stage to the next
- Reconcile this stage map with the one in crm-pipeline-attribution so attribution and operations share definitions

**Gate:** Stage list with exit criteria, timestamps, and transition owners written and loaded into the CRM.

### Step 2: Build the Lead Scoring Model
Score on fit and behavior, and keep the model explainable:
- Separate the two axes — fit (who they are — firmographics, role, company size) from behavior (what they did — pages, trials, downloads, engagement)
- Choose a scale and threshold — the score at which a lead becomes an MQL — and document why that number
- Prefer a small number of well-understood signals over a hundred weights nobody can explain; every point should be defensible to a skeptical AE
- Treat negative signals as seriously as positive ones — competitors, non-buyer roles, students
- Decay behavioral points over time; a lead who was engaged six months ago is not engaged now
- Plan a calibration loop — compare predicted quality (score) against actual outcome (meeting booked, deal won) and tune quarterly

**Gate:** Two-axis scoring model with a documented MQL threshold and a scheduled calibration loop.

### Step 3: Set MQL and SQL Criteria
Pin the definitions in writing so the teams stop arguing:
- MQL — a lead that meets a fit bar AND shows buying-intent behavior, as measured by the score or explicit criteria
- SQL — a lead sales has accepted and verified as a real opportunity with budget, authority, need, and timing signals present
- Own both definitions at revenue leadership level, not at whichever team wins the latest argument
- Define the handoff moment precisely — what changes in the CRM when a lead crosses MQL to SQL, and who does it
- Set a recycle rule — what happens to a lead sales rejects, and how it returns to nurture instead of dying

**Gate:** MQL and SQL definitions written, owned by leadership, and operationalized as CRM fields and rules.

### Step 4: Design Lead Routing Rules
Get the right lead to the right rep fast:
- Choose the routing dimension — territory, industry, product line, company size, or round-robin — based on how your sales team actually sells
- Match lead to rep within minutes, not days; speed-to-first-touch is the single biggest lever on lead quality in most teams (heuristic, verify with your data)
- Handle edge cases explicitly — no-owner regions, unknown industries, duplicates, and inbound vs outbound distinction
- Route on fit first, not geography alone — a high-fit lead from a "wrong" region usually beats a low-fit lead from a "right" one
- Automate the assignment (delegate the workflow to workflow-builder) and log who got assigned when for SLA tracking

**Gate:** Routing rules written with an owner for every case; assignment automated and time-stamped.

### Step 5: Enforce CRM Data Hygiene
Data quality is a process, not a one-time cleanup:
- Define required fields at creation — source, contact info, company, role — and block creation without them
- Standardize key fields with controlled vocabularies — no free-text source or title fields (delegate source taxonomy to utm-governance)
- Deduplicate on a documented match key (email, or company domain plus contact) and preserve source fields through merges
- Schedule recurring hygiene — a monthly stale-record sweep and a quarterly full dedupe, with an owner named for each
- Track a hygiene score — fill rate on required fields, duplicate rate, and stale-record rate — and report it alongside pipeline numbers

**Gate:** Required fields and vocabularies enforced; dedupe and hygiene sweeps scheduled with owners.

### Step 6: Design the Sales-Marketing SLA
Turn the handoff into a contract with numbers:
- Agree response time — how fast sales acts on a routed MQL (a common target is under one business hour, but set your own from capacity)
- Agree the follow-up sequence — the number and spacing of attempts before a lead is marked uncontacted and recycled
- Define quality feedback — sales rates or rejects leads with a reason; marketing reads the reasons weekly, not monthly
- Agree the volume-quality tradeoff explicitly — if marketing must hit a lead count, state the floor quality bar that can't be traded away
- Set the review cadence — a monthly SLA review where both teams look at acceptance rate, response time, and quality feedback together

**Gate:** SLA with response-time, follow-up, recycle, and feedback terms written and agreed by both teams.

### Step 7: Design the Lead-to-Revenue Process End to End
Wire the stages together into one visible flow:
- Map the full path from first touch to closed revenue, naming the system of record at each step and where data moves between tools
- Identify handoffs where records are most likely to stall or drop — usually MQL-to-SQL and SQL-to-opportunity
- Define the reporting that closes the loop — conversion by stage, velocity by stage, and source-to-revenue (delegate measurement to crm-pipeline-attribution and metrics-framework)
- Connect enrichment into the flow — score leads on enriched firmographic and intent data before routing (delegate to marketing-intelligence/account-intelligence)
- Feed outcomes back — high-quality sources get more budget; low-quality sources get diagnosis before more spend

**Gate:** End-to-end process documented with named handoffs, reporting, and a feedback loop to budget and targeting.

## Evaluation & QA

### Common Failure Modes
- MQL and SQL defined by whoever shouts loudest, so the definitions shift every quarter
- Scoring weights that no one can explain, so AEs ignore the score and route by gut
- Leads routed to empty territories and sitting unassigned for weeks
- Data hygiene done once as a project instead of as a recurring process, so the CRM rots again
- An SLA with a response-time number but no follow-up sequence, recycle rule, or quality feedback loop
- Source fields overwritten or lost in dedupe, breaking attribution downstream
- Marketing optimizing for MQL volume while sales quietly ignores the leads — no shared quality metric to surface it
