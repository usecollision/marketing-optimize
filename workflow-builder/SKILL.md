---
name: workflow-builder
category: automation
description: Design marketing automation workflows that connect tools, trigger actions, and reduce manual work.
triggers:
  - "automation workflow"
  - "automate marketing"
  - "n8n workflow"
  - "Zapier"
  - "connect tools"
  - "marketing automation"
inputs:
  - product_context
  - current_tools
  - manual_processes
  - goals
outputs:
  - workflow_designs
  - tool_recommendations
  - implementation_plan
  - roi_estimate
related_skills:
  - lead-routing
  - marketing-channels/lifecycle-sequences
  - marketing-optimize/analytics-setup
  - marketing-channels/cold-email-sequence
required_context:
  - .context/product-marketing.md
allowed_tools:
  - mcp:automation
version: 1.0.0
---

## When to Use

Invoke when:
- Doing repetitive marketing tasks manually
- Tools are disconnected (data lives in silos)
- Need to scale operations without hiring
- Setting up lead management and routing
- Building an always-on marketing machine

## Workflow

### Step 1: Process Audit
Map current manual processes:

| Process | Frequency | Time/Instance | Tool(s) | Automatable? | Impact |
|---------|-----------|---------------|---------|-------------|--------|
| Lead follow-up | Daily | 30 min | Email + CRM | Yes | High |
| Social posting | Daily | 45 min | Buffer + Canva | Partial | Medium |
| Report generation | Weekly | 2 hrs | GA4 + Sheets | Yes | Medium |
| Lead enrichment | Per lead | 5 min | LinkedIn + CRM | Yes | High |

Priority: High impact + High frequency + Currently manual = Automate first.

**Gate:** 5+ manual processes identified with time spent and automation potential.

### Step 2: Workflow Design
For each priority process, design the automation:

**Workflow structure:**
`
Trigger → Condition → Action → Output → Next Step
`

**Common marketing automation patterns:**

**1. Lead capture to CRM:**
Trigger: Form submission
→ Enrich lead (Clearbit/Apollo)
→ Score lead (based on ICP fit)
→ Route to correct sequence or sales rep
→ Add to CRM with all context

**2. Content distribution:**
Trigger: Blog post published
→ Create social variants (LinkedIn, X, newsletter)
→ Schedule posts across platforms
→ Notify team in Slack
→ Add to email newsletter queue

**3. Lead nurture:**
Trigger: Lead downloads resource
→ Wait 2 days
→ Send follow-up email
→ If opened: send case study
→ If clicked: notify sales
→ If no engagement after 3 emails: move to monthly newsletter

**4. Competitive monitoring:**
Trigger: Daily/weekly schedule
→ Check competitor websites for changes
→ Monitor their social for new campaigns
→ Alert team on significant changes
→ Log in competitive intel database

**5. Analytics reporting:**
Trigger: Monday 9am
→ Pull metrics from GA4, ads platforms, email
→ Calculate KPIs (CAC, ROAS, funnel rates)
→ Generate report
→ Send to Slack/email

**Gate:** Top 3 workflows designed with trigger, conditions, actions, and outputs.

### Step 3: Tool Selection
Choose automation platform based on needs:

| Tool | Best For | Complexity | Cost | Integration Depth |
|------|----------|-----------|------|-------------------|
| Zapier | Simple connections, non-technical teams | Low | Add README$ at scale | 5000+ apps, shallow |
| Make (Integromat) | Visual workflows, moderate complexity | Medium | Add README | 1000+ apps, deeper |
| n8n | Full control, self-hosted, complex logic | High | $ (self-host) | Unlimited, code nodes |
| Clay | Lead enrichment and outbound specific | Medium | Add README$ | Sales/marketing focused |
| Pipedream | Developer-friendly, code-first | High | $ | API-first, flexible |

Selection criteria:
- Technical capability of team
- Budget constraints
- Integration requirements
- Complexity of workflows needed
- Data privacy/hosting requirements

**Gate:** Platform selected with rationale tied to team capability and workflow needs.

### Step 4: Implementation Plan
Roll out in phases:

**Phase 1 (Week 1-2): Foundation**
- Set up automation platform
- Connect core tools (CRM, email, analytics)
- Build first workflow (highest impact, lowest complexity)
- Test thoroughly before enabling

**Phase 2 (Week 3-4): Core Workflows**
- Lead capture and routing
- Email trigger sequences
- Basic reporting automation
- Monitor for errors

**Phase 3 (Month 2): Advanced**
- Multi-step conditional workflows
- Cross-tool data sync
- Alert and notification systems
- A/B testing triggers

**Phase 4 (Month 3+): Optimization**
- AI-powered lead scoring
- Predictive triggers
- Self-optimizing sequences
- Full marketing operations automation

**Gate:** Phased plan with specific workflows per phase and success criteria.

### Step 5: Monitoring & Maintenance
Automation isn't set-and-forget:

- [ ] Error monitoring (get alerted when workflows fail)
- [ ] Weekly audit of workflow runs and success rates
- [ ] Monthly review of ROI (time saved vs cost)
- [ ] Quarterly workflow optimization (are they still relevant?)
- [ ] Documentation for team (what runs, when, why)

**Gate:** Monitoring plan with alerts, review cadence, and documentation.

## Practitioner Grounding
- **Alltomate (Zapier Platinum partner)** — "Send form submission to CRM" is not a workflow: the business rule is the automation — filters to block bad records, paths to route cases, multi-step updates, error handling so the team knows when automation needs attention. Four recurring patterns: lead intake, marketing follow-up, operations handoff, support escalation (EMPIRICAL, T2).
- **Olostep** — use native in-app automation first when a flow starts and ends in one tool (data never leaves the ecosystem); connectors (Zapier/Make/n8n) are middleware; choose tooling by complexity, event scale, governance, data origin (FRAMEWORK, T2).
- **n8n community + template library** — 3,400+ marketing workflow templates; recurring patterns: lead enrichment, competitive monitoring (scheduled crawl → diff → alert only on material change), content distribution; strong error-handling culture (error triggers, retries, recovery) (HEURISTIC, T2/T3).
- **Zapier / Clearbit** — marketing-ops automation patterns and enrichment-first lead intake (form → enrich → score → route) (TACTIC, T2).

## Decision Rules
1. IF the underlying process is broken THEN fix it before automating — automation bakes in the breakage (Alltomate, HEURISTIC, T2).
2. IF a workflow has no error path THEN add error trigger + retries + alerting before enabling — silent failures lose data (n8n/Alltomate, EMPIRICAL, T2).
3. IF the flow starts and ends inside one tool THEN use that tool's native automation, not a connector (Olostep, HEURISTIC, T2).
4. IF automating lead intake THEN include filters for bad records and required-field checks (Alltomate, EMPIRICAL, T2).
5. IF prioritizing what to automate THEN pick high-frequency × high-impact × currently-manual (universal, HEURISTIC, T2).
6. IF a workflow runs unattended THEN give it an owner, documentation, and a quarterly relevance review (Zapier/Olostep, HEURISTIC, T2).
7. IF data moves across systems THEN define the source-of-truth key — cross-tool sync without keys creates duplicates (Clearbit/Alltomate, EMPIRICAL, T2).

## Metrics
- Primary: workflow run success rate; error rate per workflow; time saved (measured, not estimated).
- Secondary: automation coverage (manual-to-automated task ratio), tasks completed per week, exceptions needing human review.
- Guardrails: silent-failure count (should be zero — every failure alerts), data-quality incidents into CRM, compliance (GDPR/CAN-SPAM) violations.
- Timebox: weekly run audit, monthly ROI review, quarterly relevance review (kill or refactor stale workflows).

## Practitioner Failure Modes
- Automating broken processes (Alltomate, universal).
- Silent failures with no alerting — data loss discovered weeks later (n8n/Alltomate).
- No filters on intake → garbage records into the CRM (Alltomate).
- Undocumented automations — bus factor 1; the workflow dies with its builder (Zapier).
- Automation debt: the same manual process repeated weekly that nobody automated (Olostep).
- Over-engineering day one: complex multi-path flows before simple ones are proven (Olostep).
- Workflows outliving their purpose — no quarterly relevance review (universal).

## Sources
1. Alltomate — Zapier Workflow Examples: 4 Real Automation Patterns | alltomate.com/blogs/zapier-workflow-examples | T2 | 2026-08-15
2. Alltomate — n8n Error Handling: Error Triggers, Retries & Recovery | alltomate.com/blogs/n8n-error-handling | T2 | 2026-08-15
3. Olostep — Workflow Automation: Examples, Tools & Best Practices | olostep.com/blog/workflow-automation | T2 | 2026-08-15
4. n8n — Top Marketing Automation Workflows | n8n.io/workflows/categories/marketing | T2 | 2026-08-15
5. Zapier — 64 Zapier examples for marketers & creatives | zapier.com/blog/automate-new-zapier-products-free | T2 | 2026-08-15
6. Clearbit — How to automate marketing ops workflows with Clearbit and Zapier | clearbit.com/resources/guides/Zapier-automate-marketing-ops-workflows | T2 | 2026-08-15
7. Panel: practitioner-intelligence/domains/optimize-longtail/automation.md | T2 | 2026-08-15

## Evaluation & QA

### Common Failure Modes
- Automating broken processes (fix the process first, then automate)
- Over-engineering from day one (start simple, add complexity as needed)
- No error handling (workflows fail silently, data gets lost)
- Not testing with real data before going live
- Automation without documentation (only one person knows how it works)
- Ignoring data privacy (GDPR, CAN-SPAM compliance in automated flows)