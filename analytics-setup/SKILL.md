---
name: analytics-setup
category: analytics
description: Implement GA4 + GTM tracking the right way — event taxonomy, dataLayer architecture, tracking plan, conversion events, and a debugging workflow.
triggers:
  - "set up GA4"
  - "set up Google Tag Manager"
  - "event tracking plan"
  - "dataLayer not working"
  - "conversion tracking broken"
  - "tracking implementation"
  - "GTM debugging"
inputs:
  - business_model
  - funnel_stages
  - metrics_framework
  - existing_tracking_state
  - consent_requirements
outputs:
  - event_taxonomy
  - tracking_plan
  - datalayer_spec
  - ga4_configuration
  - qa_checklist
related_skills:
  - metrics-framework
  - attribution-model-selection
  - funnel-analysis
  - workflow-builder
  - mmm-incrementality
  - marketing-paid/performance-reporting
required_context:
  - .context/product-marketing.md
allowed_tools:
  - mcp:analytics
version: 1.0.0
---

## When to Use

Invoke when:
- Setting up GA4 and/or GTM from scratch for a site or app
- Existing tracking is messy, duplicated, or untrustworthy
- Leadership wants conversion data that answers real business questions
- Launching a new funnel, checkout, or signup flow that needs events
- Preparing for A/B testing or attribution work that requires clean data
- Auditing a container before handing it to an agency or new hire

## Workflow

### Step 1: Measurement Strategy
Define what analytics must answer before touching any tool:
- List the 5-10 business questions analytics must answer (from metrics-framework)
- Map each question to the events, parameters, or reports that answer it
- Define conversion events — the 3-8 actions that actually matter to revenue
- Note consent requirements (GDPR/CCPA) and whether tracking must degrade gracefully

Tracking that isn't tied to a question is noise. Every event must earn its place.

**Gate:** Business questions, conversion events, and consent constraints written down and agreed with stakeholders.

### Step 2: Event Taxonomy Design
Design the naming system before configuring anything:
- Naming convention — snake_case, `object_action` (e.g. `signup_submitted`, `pricing_viewed`)
- Event classes — automatically collected, enhanced measurement, recommended, custom
- Consistent parameters per event — method, value, currency, item type
- Never reuse one event name for two different meanings

Build the taxonomy as a table:

| Event name | Trigger | Parameters | Where fired | Priority |
|---|---|---|---|---|
| signup_submitted | Form submit success | method, plan | Signup page | P0 |
| checkout_completed | Purchase confirmed | value, currency, items | Thank-you page | P0 |

**Gate:** Full taxonomy table covering every funnel stage, signed off by product and marketing.

### Step 3: dataLayer Architecture
Define the dataLayer as the single source of truth:
- Push structured objects on meaningful events, not on every click
- Follow the official GA4 ecommerce spec for purchase/item events
- Push user properties (plan, role, segment) at init, not per event
- Never scrape the DOM in GTM when a dataLayer push is possible
- Document every push — name, shape, and timing — in the tracking plan

Example push pattern:

```
dataLayer.push({
  event: 'signup_submitted',
  signup_method: 'email',
  plan: 'pro',
  user_id: 'usr_123'
});
```

**Gate:** dataLayer spec written — every event, its shape, and where it fires.

### Step 4: GTM Container Setup
Configure the container with discipline:
- One container per environment; test staging before production
- Naming convention for tags, triggers, and variables (e.g. `GA4 - Purchase`)
- Prefer dataLayer variables over auto-event variables and CSS selectors
- Folders per product area; pause tags instead of deleting when retired
- Consent mode configured so tags respect user consent signals
- Publish with version names and notes describing what changed and why

**Gate:** Container follows conventions, passes preview in staging, and version history is legible.

### Step 5: GA4 Configuration
Configure the property so data is trustworthy:
- One property per business with separate data streams for site and app
- Filter internal traffic and bot traffic via data filters
- Configure cross-domain tracking for multi-domain funnels
- Mark key events (conversions) matching the taxonomy P0s
- Register custom dimensions/metrics for taxonomy parameters
- Set data retention appropriately and link Google Ads + BigQuery export

**Gate:** GA4 reproduces a known answer (e.g. a test purchase shows up as a conversion).

### Step 6: Debugging & QA
Verify end to end before declaring done:
- GTM Preview mode on staging, then production
- GA4 DebugView to confirm events and parameters in near real time
- BigQuery export to validate the dataLayer payload shape
- QA checklist — every P0 event fired, parameters populated, no duplicates
- Cross-check GA4 counts against backend or CRM records for critical events
- Schedule a monthly data health check (missing events, anomalies, dupes)

**Gate:** QA checklist passes; backend cross-check within an accepted, documented tolerance.

### Step 7: Documentation & Governance
Make tracking maintainable:
- Tracking plan document with owner, changelog, and a request process
- Naming decisions documented so future tags stay consistent
- Change review — who approves new events before they ship
- Onboarding notes so a new owner can maintain the setup

**Gate:** Tracking plan published with a named owner; someone other than you can maintain it.

## Evaluation & QA

### Common Failure Modes
- Tracking everything, answering nothing (events not tied to questions)
- Duplicate tags firing the same event (double-counted conversions)
- DOM scraping that breaks with every redesign
- Events named by developer whim instead of taxonomy
- Skipping the backend cross-check — GA4 says 100 signups, CRM says 60
- No consent handling, breaking GDPR compliance
- Changing tracking without updating the tracking plan
