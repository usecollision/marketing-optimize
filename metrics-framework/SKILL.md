---
name: metrics-framework
category: analytics
description: Define the right marketing metrics for your stage, goals, and channels with clear targets and reporting cadence.
triggers:
  - "marketing metrics"
  - "what should we measure"
  - "KPIs"
  - "marketing dashboard"
  - "how do we know if marketing is working"
inputs:
  - product_context
  - business_stage
  - active_channels
  - goals
outputs:
  - metrics_hierarchy
  - dashboard_design
  - reporting_cadence
  - target_benchmarks
related_skills:
  - analytics-setup
  - funnel-analysis
  - marketing-optimize/attribution-model-selection
  - marketing-intelligence/growth-strategy
required_context:
  - .context/product-marketing.md
  - .context/project-context.md
allowed_tools:
  - mcp:analytics
version: 1.0.0
---

## When to Use

Invoke when:
- Setting up marketing measurement from scratch
- Drowning in data but can't tell what's working
- Leadership asking "is marketing working?"
- Switching from vanity metrics to meaningful ones
- Onboarding a new marketing hire who needs clarity

## Workflow

### Step 1: North Star Identification
Every marketing org needs ONE north star metric tied to revenue:

| Stage | Typical North Star | Why |
|-------|-------------------|-----|
| Pre-PMF | Activation rate or weekly active users | Proves value |
| Early growth | MRR or new customers/month | Revenue momentum |
| Scaling | Net Revenue Retention or CAC payback | Efficient growth |
| Mature | Market share or LTV:CAC ratio | Sustainable moat |

Your north star: ____________
Current value: ____________
Target: ____________ by ____________

**Gate:** One north star defined with current state and target.

### Step 2: Metrics Hierarchy
Build a tree from north star down to leading indicators:

`
North Star: [metric]
├── Acquisition Metrics
│   ├── Traffic (by source)
│   ├── Lead volume
│   ├── CAC (by channel)
│   └── Pipeline value
├── Activation Metrics
│   ├── Signup → Activation rate
│   ├── Time to value
│   └── Feature adoption
├── Revenue Metrics
│   ├── Conversion rate (lead → customer)
│   ├── ACV / ARPU
│   ├── Sales velocity
│   └── Win rate
├── Retention Metrics
│   ├── Churn rate
│   ├── NRR (Net Revenue Retention)
│   └── Engagement scores
└── Efficiency Metrics
    ├── CAC payback period
    ├── LTV:CAC ratio
    ├── Marketing efficiency ratio (MER)
    └── ROAS (by channel)
`

**Gate:** Metrics tree maps from north star through each funnel stage.

### Step 3: Channel-Specific Metrics
For each active channel, define what to track:

| Channel | Input Metrics | Output Metrics | Efficiency |
|---------|--------------|----------------|-----------|
| SEO | Rankings, indexed pages, backlinks | Organic traffic, organic leads | Cost per organic lead |
| Paid | Spend, impressions, clicks | Conversions, revenue | ROAS, CAC |
| Email | List size, sends, opens, clicks | Conversions, revenue | Revenue per email |
| Content | Published pieces, traffic per post | Leads from content | Cost per content lead |
| Social | Posts, engagement rate, followers | Traffic, DMs, leads | Time per lead |
| Outbound | Emails sent, reply rate | Meetings booked, pipeline | Cost per meeting |

**Gate:** Each active channel has input/output/efficiency metrics defined.

### Step 4: Targets & Benchmarks
Set targets for each metric:

| Metric | Current | Target | Timeline | Benchmark |
|--------|---------|--------|----------|-----------|
| [metric] | [value] | [goal] | [date] | [industry avg] |

Rules for good targets:
- Based on historical trend + growth rate needed
- Realistic given resources and market
- Time-bound (not "improve" but "reach X by Y")
- Broken into monthly/weekly milestones

**Gate:** Every metric has a specific target with timeline.

### Step 5: Reporting Cadence
Define what gets reported when:

| Cadence | Metrics | Audience | Format |
|---------|---------|----------|--------|
| Daily | Spend, leads, key alerts | Marketing team | Dashboard |
| Weekly | Channel performance, pipeline | Marketing + Sales | Brief report |
| Monthly | Full funnel, CAC, LTV, trends | Leadership | Deck/doc |
| Quarterly | Strategy review, benchmarks, roadmap | Exec team | Board-level |

**Gate:** Reporting rhythm defined with audience and format for each cadence.

## Evaluation & QA

### Common Failure Modes
- Tracking everything (noise drowns signal)
- Vanity metrics as KPIs (followers, impressions without conversion)
- No targets (measuring without knowing what good looks like)
- Measuring only lagging indicators (by the time you see it, it's too late)
- Different definitions across teams (what counts as a "lead"?)
- No action triggers (what do you DO when a metric drops?)