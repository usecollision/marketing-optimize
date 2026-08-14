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

## Practitioner Grounding & Decision Rules

Built from Peep Laja (PXL), Craig Sullivan, Brian Massey, Ben Labay, plus win-rate evidence from NN/g, DRIP, and growwithba. Full research: practitioner-intelligence/syntheses/cro.md.

- **Score before debate** (Laja/Massey — FRAMEWORK, T1): objective scoring (PXL or Evidence/Impact/Effort/Traffic/ROI) removes opinion-only ideas before they consume capacity. Score against evidence provenance: measured data > user research > analogy > hunch.
- **Win-rate band as a calibration diagnostic** (synthesis — HEURISTIC, T2): a healthy program wins 20-40% of tests. Below 20%: hypotheses are cosmetic/researchless. Above 40%: tests are too safe. Track win rate by confidence bucket as the calibration loop (existing Step 4 — now with a target band).
- **Backlog mix** (Labay/Kohavi — FRAMEWORK, T1): quick wins + bets + moonshots, with pipeline coverage of 3-5x sustain rate (existing). Kohavi's finding: most companies run too few tests — velocity is a goal in itself, but trustworthiness must scale with volume.

Decision rules:
1. IF a hypothesis has no evidence source (no data, no user research, no documented analogy) THEN score its Confidence ≤ 2 regardless of enthusiasm — opinion-only ideas drop below the build line (Laja — FRAMEWORK, T1).
2. IF win rate is below ~20% over the last 20 tests THEN require research (analytics/qualitative) before new hypotheses enter the backlog — the pipeline is generating cosmetic tests (synthesis — HEURISTIC, T2).
3. IF win rate is above ~40% THEN the backlog is too safe — add bigger-swing bets (synthesis — HEURISTIC, T2).
4. IF a high-Confidence hypothesis loses THEN recalibrate: the confidence model overrated the evidence class — downgrade that evidence class for a quarter (existing calibration loop, now with attribution to the evidence class, Laja — FRAMEWORK, T1).
5. IF an experiment's result is inconclusive THEN log it as inconclusive (a valid outcome) and re-propose with a larger MDE or different mechanism — never rewrite the outcome (Kohavi/Georgiev — EMPIRICAL, T1).
6. IF capacity allows only one test per sprint per owner THEN prioritize pipeline coverage over perfect scoring — 3-5x scored backlog for each sustained test/month (existing + Labay — FRAMEWORK, T1).

## Sources

1. Peep Laja, ResearchXL + PXL frameworks | CXL blog | tier 1 | 2026-08-14
2. Craig Sullivan, hypothesis format and failure-mode talks | cxl.com / conference decks | tier 1 | 2026-08-14
3. Brian Massey, Evidence/Impact/Effort/Traffic/ROI scoring | Conversion Sciences | tier 2 | 2026-08-14
4. Ben Labay, experimentation program ops (Speero Phase Gate) | speero.org | tier 2 | 2026-08-14
5. Ronny Kohavi et al., *Trustworthy Online Controlled Experiments* (experiment velocity, OEC) | exp-platform.com | tier 1 | 2026-08-14
6. Win-rate data: NN/g reports, DRIP conversion benchmarks, growwithba discipline studies | tier 3 | 2026-08-14

## Evaluation & QA

### Common Failure Modes
- Scoring by enthusiasm instead of evidence (Confidence inflated)
- Backlog full of moonshots and no quick wins — velocity stalls
- Switching scoring models every quarter — nothing is comparable
- Tests launched without owners or without predicted effect sizes
- Measuring activity (tests run) instead of impact (cumulative lift)
- The same hypothesis re-proposed every month because nobody recorded the outcome
