---
name: attribution-model-selection
category: attribution
description: Choose and implement the right attribution model based on your stage, channels, and data maturity.
triggers:
  - "attribution"
  - "which channel is working"
  - "attribution model"
  - "how to measure channel contribution"
  - "incrementality"
  - "MMM"
inputs:
  - product_context
  - channels_active
  - data_maturity
  - budget_level
outputs:
  - model_recommendation
  - implementation_plan
  - measurement_framework
  - limitations_doc
related_skills:
  - tracking-setup
  - marketing-optimize/metrics-framework
  - marketing-paid/paid-strategy
required_context:
  - .context/product-marketing.md
allowed_tools:
  - mcp:analytics
version: 1.0.0
---

## When to Use

Invoke when:
- Spending on multiple channels and don't know what's working
- Need to decide where to allocate budget
- Leadership asking for channel ROI numbers
- CAC is rising and need to understand why
- Moving from single-channel to multi-channel

## Workflow

### Step 1: Assess Data Maturity
Understand what you can actually measure:

| Level | Data Available | Best Model | Typical Stage |
|-------|--------------|-----------|---------------|
| 1 | Basic analytics, no CRM | Last-touch (default) | Pre-seed, </mo spend |
| 2 | CRM + analytics connected | First-touch + Last-touch comparison | Seed, -20k/mo |
| 3 | Multi-touch data, UTMs, CRM events | Multi-touch (linear, position-based) | Series A, -100k/mo |
| 4 | Large datasets, statistical capability | MMM, incrementality tests | Growth, +/mo |

**Gate:** Data maturity level identified honestly (don't over-engineer).

### Step 2: Model Selection
Choose based on your situation:

**Last-Touch Attribution:**
- Best for: Direct response, single-channel dominance, early stage
- Pros: Simple, actionable, easy to implement
- Cons: Ignores awareness and consideration touches
- When to use: <3 channels, </mo spend

**First-Touch Attribution:**
- Best for: Understanding top-of-funnel, awareness investment
- Pros: Values discovery, helps justify brand spend
- Cons: Ignores what actually closed the deal
- When to use: Alongside last-touch for comparison

**Multi-Touch (Linear/Position-Based):**
- Best for: Multiple touchpoints, longer sales cycles
- Pros: More fair distribution, better budget decisions
- Cons: Requires complete tracking, still rule-based
- When to use: B2B SaaS, 3+ channels, + spend

**Data-Driven/MMM:**
- Best for: Large spend, many channels, statistical rigor
- Pros: Accounts for correlation vs causation, cross-channel effects
- Cons: Requires large data sets, statistical expertise
- When to use: +/mo spend, dedicated data team

**Incrementality Testing:**
- Best for: Validating specific channel contribution
- Pros: Causal (not just correlational), definitive answers
- Cons: Requires holdout groups, takes time, loses some revenue during test
- When to use: Any budget, to validate big channel decisions

**Gate:** Model selected with honest assessment of data/resource constraints.

### Step 3: Implementation Plan
For chosen model:

**Basic (Last/First Touch):**
- UTM parameters on all links (source, medium, campaign, content, term)
- GA4 configured with conversion events
- CRM captures first-touch and last-touch source
- Monthly comparison report

**Multi-Touch:**
- All Basic requirements plus:
- Touchpoint logging in CRM (every interaction timestamped)
- Cookie/identity resolution across devices
- Custom attribution model in analytics
- Weighted credit allocation rules defined

**Advanced (MMM/Incrementality):**
- All above plus:
- Spend data by channel by day/week
- External factors (seasonality, competitor activity, PR)
- Statistical model (regression-based or Bayesian)
- Geo-holdout or time-based incrementality tests designed

**Gate:** Implementation plan with specific tools, tracking requirements, and timeline.

### Step 4: Practical Framework (MER + Channel CAC)
Regardless of model, always track:

**Marketing Efficiency Ratio (MER):** Total Revenue / Total Marketing Spend
- Simple, holistic, hard to game
- Trend over time matters more than absolute number
- Rising MER = marketing getting more efficient

**Channel CAC:** Spend per Channel / Customers from Channel
- Even imperfect attribution gives directional signal
- Compare trends, not exact numbers
- Use for relative allocation, not absolute truth

**Blended CAC:** Total Sales + Marketing Spend / New Customers
- The number that matters most
- Must be < LTV/3 for healthy business
- Track monthly, set targets

**Gate:** MER + Channel CAC + Blended CAC calculated and tracked.

## Evaluation & QA

### Common Failure Modes
- Perfect attribution is a myth (accept directional accuracy)
- Over-crediting last touch (ignores all awareness work)
- Building complex models without sufficient data
- Not accounting for iOS privacy / cookie deprecation
- Attribution replacing judgment (it informs decisions, doesn't make them)
- Ignoring offline and word-of-mouth (self-reported attribution helps)