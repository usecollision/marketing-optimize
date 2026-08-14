---
name: experiment-prioritization
category: experimentation
description: Prioritize experiments with ICE, RICE, PIE, and PXL scoring — hypothesis library, backlog management, and testing velocity planning.
triggers:
  - "prioritize experiments"
  - "ICE score"
  - "RICE score"
  - "PIE framework"
  - "PXL framework"
  - "experiment backlog"
  - "testing roadmap"
inputs:
  - hypothesis_list
  - team_capacity
  - business_goals
outputs:
  - scored_backlog
  - velocity_plan
  - hypothesis_library
related_skills:
  - ab-testing
  - cro-audit
  - funnel-analysis
  - landing-page-optimization
  - marketing-paid/creative-testing
required_context:
  - .context/product-marketing.md
allowed_tools: none
version: 1.0.0
---

## When to Use

Invoke when:
- The testing backlog is a flat list and nobody knows what to run next
- The team debates "what should we test" every sprint
- Testing velocity is low and you need a plan to raise it
- Leadership wants a testing roadmap with expected impact

## Workflow

### Step 1: Hypothesis Library Intake
Standardize how ideas enter the system:
- One entry per hypothesis, written in ab-testing format
- Required fields — problem, hypothesis, metric, evidence, source (data, research, anecdote)
- Statuses — Icebox, Backlog, Ready, In Test, Shipped, Killed
- Review cadence — triage new ideas weekly, prune dead ones monthly

**Gate:** Intake template live; every new idea enters in a consistent format.

### Step 2: Score with ICE / RICE
Score each hypothesis on evidence and business logic:

ICE (Impact, Confidence, Ease — 1-10 or 1-5):

```
Score = Impact * Confidence * Ease
```

RICE adds reach:

```
Score = (Reach * Impact * Confidence) / Effort
```

- Reach — users affected per period (use numbers, not guesses)
- Impact — expected lift if the hypothesis is true (tie to the predicted effect from ab-testing)
- Confidence — strength of evidence (measured data > user research > analogy > hunch)
- Effort — person-days, not "hard" or "easy"

**Gate:** Every hypothesis scored with explicit values for each dimension.

### Step 3: Score with PIE or PXL
Upgrade scoring when the backlog is large or stakeholders want rigor:

PIE (Potential, Importance, Ease — from Widerfunnel):
- Potential — how much can this page or area improve?
- Importance — traffic volume and business value
- Ease — technical simplicity of testing here

PXL (from CXL's framework) — score against pre-defined questions (data availability, clarity of hypothesis, impact potential) instead of gut feel. PXL trades speed for calibration; use it when scores keep drifting.

Pick one model and stick with it for a quarter; switching models mid-cycle resets comparability.

**Gate:** A single scoring model selected, documented, and applied consistently.

### Step 4: Calibrate Scores
Scores drift — recalibrate deliberately:
- Anchor the scale with 2-3 reference hypotheses (a known quick win, a known moonshot)
- Require evidence for high Confidence scores — name the data source
- Recalibrate after results come in — did 9/10-Confidence tests actually win?
- Track win rate by confidence bucket as a calibration loop (heuristic practice)

**Gate:** Anchor examples set; a process exists to compare predicted vs actual outcomes.

### Step 5: Backlog Management
Keep the queue healthy:
- T-shirt size effort as you refine (S/M/L mapped to person-days)
- Kill duplicates and stale ideas quarterly
- Maintain a mix — quick wins, bets, and moonshots — so velocity and upside both exist
- Assign an owner to each Ready item

**Gate:** Backlog deduplicated, sized, and mixed across risk categories.

### Step 6: Velocity & Capacity Planning
Plan how many tests you can actually run:
- Capacity per tester — one meaningful experiment per sprint is a healthy assumption; verify against your history
- Expected win rate for planning — use your own historical rate, not an assumed one
- Pipeline coverage — to sustain N tests/month, keep roughly 3-5x N scored hypotheses in the backlog (heuristic)
- Big bets need design and dev lead time — plan them two sprints ahead

**Gate:** Velocity plan shows tests/month, capacity by owner, and pipeline coverage ratio.

### Step 7: Roadmap & Reporting
Make the program legible:
- Monthly review — wins, losses, learnings, win rate trend
- Quarterly recalibration — scoring model review, metric targets, backlog health
- Learning library — every test's result searchable, especially the losses
- Report velocity and cumulative impact, not just the latest shiny win

**Gate:** Reporting cadence defined; learning library is the source of truth for past tests.

## Evaluation & QA

### Common Failure Modes
- Scoring by enthusiasm instead of evidence (Confidence inflated)
- Backlog full of moonshots and no quick wins — velocity stalls
- Switching scoring models every quarter — nothing is comparable
- Tests launched without owners or without predicted effect sizes
- Measuring activity (tests run) instead of impact (cumulative lift)
- The same hypothesis re-proposed every month because nobody recorded the outcome
