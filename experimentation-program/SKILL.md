---
name: experimentation-program
category: experimentation
description: Run an experimentation program — portfolio strategy, velocity tracking, learning library, guardrail metrics, experiment culture, and OKR alignment.
triggers:
  - "experimentation program"
  - "experiment velocity"
  - "learning library"
  - "guardrail metrics"
  - "experiment culture"
  - "testing program strategy"
  - "experiment OKRs"
  - "shipping criteria"
inputs:
  - experiment_backlog
  - test_results_history
  - okr_targets
  - team_capacity
  - guardrail_definitions
outputs:
  - program_strategy
  - velocity_dashboard
  - learning_library
  - documentation_standard
  - okr_experiment_map
  - ship_criteria
related_skills:
  - ab-testing
  - experiment-prioritization
  - metrics-framework
  - dashboard-design
  - cro-audit
  - benchmark-frameworks
  - marketing-paid/creative-testing
required_context:
  - .context/product-marketing.md
allowed_tools: none
version: 1.0.0
---

## When to Use

Invoke when:
- Individual tests run fine but the program as a whole isn't moving the business
- You can't answer "what did we learn last quarter" without digging through old tickets
- Testing velocity is flat and nobody owns it
- Winning tests aren't shipping — they stall in a queue or get quietly dropped
- Leadership asks how experiments connect to OKRs and nobody has a map
- The team celebrates wins but has no record of losses, so the same mistakes repeat

## Workflow

### Step 1: Define the Program Strategy
Decide what the program is for before optimizing how it runs:
- Write the program's one-line purpose — what business metric does testing move, and how fast
- Name the owner — one person accountable for velocity, quality, and learning capture
- Set the ambition — how many tests per month, and what fraction should be "bets" vs "quick wins" vs "moonshots" (borrow the mix logic from experiment-prioritization)
- Agree what counts as a test — a controlled A/B with a pre-registered primary metric, not a "we shipped it and it felt good"
- Define the governance tier — which tests need review before launch, which can self-serve

**Gate:** Program purpose, owner, velocity ambition, and test definition written and agreed.

### Step 2: Connect Experiments to OKRs
Map every experiment to a business outcome so the program is auditable:
- List the active OKRs (or top-line business metrics) the program can plausibly influence
- For each OKR, name the input metric a test can move — an OKR like "raise activation" maps to a signup or onboarding metric, not a bounce-rate proxy
- Tag every hypothesis in the backlog with its target OKR and input metric
- Flag the gap — OKRs with no tests aimed at them, and tests aimed at no OKR
- Re-map quarterly as OKRs change; an orphaned test is usually dead weight

**Gate:** Every in-flight and planned test tagged to an OKR; gaps named and triaged.

### Step 3: Track Experiment Velocity
Measure the program's health, not just its output:
- Core velocity metrics — tests launched per month, tests reaching a decision per month, median days from hypothesis to decision, win rate (use your own history, not an assumed rate)
- Compute cumulative impact — sum the lift of shipped winners (delegate the math to ab-testing for how each lift was measured)
- Track pipeline coverage — to sustain N decisions per month, keep roughly 3-5x N scored hypotheses in the backlog (heuristic, verify against your history)
- Watch the funnel between stages — ideas to ready, ready to live, live to decision — and name where tests pile up
- Report velocity trend over time, not a single month's number

**Gate:** Velocity dashboard live with launch rate, decision rate, cycle time, and coverage ratio.

### Step 4: Stand Up the Learning Library
Make every result — especially losses — searchable and reusable:
- One record per completed test with fixed fields — hypothesis, mechanism tested, metric, predicted vs actual lift, decision (ship / iterate / kill), and the key learning
- Write learnings as generalizable statements, not "variant B won" — capture the mechanism, e.g. "removing social proof above the fold reduced trust on this audience"
- Require a learning for losses too; a killed test with a clear mechanism is worth as much as a win
- Index by surface (page, funnel stage, audience) and by mechanism so future hypotheses can cite past results
- Link the library to the experiment-prioritization backlog so every new hypothesis cites the learning it builds on

**Gate:** Every completed test has a record; losses carry a mechanism-level learning; library is searchable by surface and mechanism.

### Step 5: Set Documentation Standards for Shipped Learnings
Ship a test into the library with the same rigor you'd ship code:
- Required before a test is "closed" — hypothesis text, sample size and duration actually run, guardrail results, primary-metric CI, decision rationale
- Record what changed mid-test (page edits, traffic shifts) and flag any test that should be discounted or re-run
- Distinguish "shipped" from "learned" — a test can teach a mechanism without a rollout; a rollout without a learning is a miss
- Store the analysis artifact (raw numbers, segments) alongside the one-paragraph summary, never just the summary
- Set a review gate — a learning isn't "shipped" until a second person can read it and restate the mechanism

**Gate:** Documentation standard written; shipped learnings pass a second-reader review before closing.

### Step 6: Set Guardrail and Quality Metrics
Protect the business and the program's credibility:
- Define program-level guardrails — the metrics that must not degrade even if a test "wins" (revenue per visitor, retention proxies, churn, refunds)
- Set a quality floor — minimum sample size and duration before any test can be declared (delegate thresholds to ab-testing)
- Track decision quality over time — the share of shipped tests that held their lift after rollout, and win rate by confidence bucket (heuristic calibration loop)
- Watch for SRM and bug rates — a program with frequent sample-ratio mismatches is shipping noise
- Publish guardrail status in the same review where wins are celebrated

**Gate:** Guardrail metrics named with thresholds; quality and calibration metrics tracked and reported.

### Step 7: Build Experiment Culture
Make testing a habit, not a campaign:
- Run a fixed cadence — weekly backlog triage, biweekly readout, monthly program review with leadership
- Celebrate learnings as loudly as wins — publicize a "best learning of the month," not just "biggest lift"
- Make it safe to be wrong — no career risk for a well-run test that lost; risk lives in untested opinions, not failed experiments
- Push hypotheses to be mechanism-driven — "because" is mandatory, not optional (enforce the ab-testing format)
- Give every team a path in — PMs, engineers, and support can all propose tests, but all enter through the same intake and scoring

**Gate:** Cadences scheduled; learning celebrations and hypothesis quality visibly reinforced each cycle.

### Step 8: Define Shipping Criteria for Winning Tests
A significant lift is necessary but not sufficient to ship:
- Require the confidence interval to exclude the minimum detectable effect in the predicted direction (delegate to ab-testing)
- Check guardrails first — a winner that degrades a guardrail metric does not ship
- Weigh rollout cost against expected impact — a +1% lift that costs a quarter of engineering may not be worth it
- Confirm the mechanism generalizes — one page's win doesn't automatically apply elsewhere; treat extension as a new test
- Write the ship decision with its rationale into the learning library, then hand the rollout to the owning team

**Gate:** Shipping checklist applied to every winner; decisions recorded with rationale in the library.

## Evaluation & QA

### Common Failure Modes
- Celebrating velocity (tests run) while cumulative business impact stays flat
- A learning library full of wins and empty of losses, so failures repeat
- Tests tagged to OKRs retroactively to make the program look aligned, rather than planned against them
- Shipping significant-but-tiny lifts that don't clear practical significance or rollout cost
- Guardrail metrics defined but never checked at ship time
- No owner for the program, so velocity and learning capture drift until someone asks
- The same hypothesis re-proposed every quarter because the library isn't searched before intake
