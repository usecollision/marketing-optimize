---
name: signup-flow
category: cro
description: Optimize signup and onboarding — form fields, activation metric definition, activation milestones, onboarding friction, and email handoffs.
triggers:
  - "signup flow optimization"
  - "activation rate low"
  - "onboarding friction"
  - "define activation metric"
  - "signup form too long"
  - "time to value"
  - "new user onboarding"
inputs:
  - current_signup_flow
  - activation_data
  - onboarding_steps
  - email_sequences
outputs:
  - activation_metric
  - milestone_map
  - friction_recommendations
  - handoff_plan
related_skills:
  - funnel-analysis
  - landing-page-optimization
  - ab-testing
  - experiment-prioritization
  - metrics-framework
  - marketing-channels/lifecycle-sequences
  - marketing-messaging/email-copy
required_context:
  - .context/product-marketing.md
allowed_tools:
  - mcp:analytics
version: 1.0.0
---

## When to Use

Invoke when:
- Signup-to-activation rate is the funnel's biggest leak
- Users sign up but never reach the moment of value
- The activation metric is undefined or contested
- Onboarding feels like friction and nobody owns it
- Email and in-product handoffs are disjointed

## Workflow

### Step 1: Map the Current Path
Document the real flow, not the intended one:
- Every step from signup click to first value, including emails and push
- Time per step and drop-off per step, from analytics
- Note dead ends — account created, then nothing guides the user
- Include platform realities (app download, invite flows, workspace setup)

**Gate:** Step-by-step map with drop-off rates and time stamps per step.

### Step 2: Define the Activation Metric
Activation means the user reached value, not that they logged in:
- Find the "aha moment" — the earliest action that correlates with retention
- Test candidate milestones against downstream behavior (does doing X predict week-4 retention?)
- Define activation as a binary event or a milestone reached within N days
- Write the definition down — one sentence, one event, agreed by product and marketing

Do not accept vanity proxies (account created, first login) as activation.

**Gate:** One activation event defined, with evidence it correlates with retention.

### Step 3: Signup Form Field Audit
Every field is a gate:
- List each field, its format, and its business owner
- Challenge each — is it needed at signup, later, or never?
- Prefer progressive profiling — ask later once the user is committed
- Weigh social login trade-offs — faster signup vs email ownership and data quality
- Test the flow on mobile, where most signups happen

**Gate:** Each field justified or cut; a shorter signup flow proposed with evidence.

### Step 4: Onboarding Friction Diagnosis
Find what blocks the path to activation:
- Time-to-value — how many minutes and steps from signup to aha moment?
- Setup requirements — invites, installs, integrations, imports that stall progress
- Empty states and dead ends after signup
- Guidance quality — checklist, progress indicator, contextual tips, or silence?
- Watch session recordings of new users to log the exact moment intent dies

**Gate:** Friction inventory with severity ratings, tied to specific steps in the map.

### Step 5: Email & Notification Handoffs
Design the out-of-product safety net:
- Trigger map — which drop-off points get which email or push, and when
- Welcome email — deliver the next step within minutes, not a brochure
- Activation nudges timed to observed drop-off windows, not a fixed drip
- Hand-off criteria — when does the user leave onboarding and enter lifecycle nurture?
- Honor opt-out preferences or burn trust

**Gate:** Trigger map showing every drop-off point paired with a message and timing.

### Step 6: Measurement & Test Plan
Make improvement measurable:
- Primary metric — signup-to-activation rate (or time-to-activation)
- Secondary metrics — signup completion, day-1 retention, support contact rate
- Convert top friction hypotheses into ab-testing format
- Sequence tests one per flow stage to keep attribution clean
- Re-measure baseline after each shipped change

**Gate:** Measurement plan with primary metric, baseline, and sequenced test hypotheses.

## Evaluation & QA

### Common Failure Modes
- Optimizing signup completion while activation stays broken (signups up, revenue flat)
- Activation metric chosen by opinion instead of retention correlation
- Removing fields the sales team silently depends on
- Onboarding that teaches features instead of driving to the aha moment
- Email sequences that ignore where users actually drop off
- Testing too many signup changes at once and misattributing the win
