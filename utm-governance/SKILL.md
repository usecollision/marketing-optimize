---
name: utm-governance
category: analytics
description: Run UTM and campaign operations cleanly — taxonomy design, naming conventions, source-medium governance, automation rules, reporting hygiene.
triggers:
  - "UTM parameters"
  - "UTM taxonomy"
  - "campaign naming convention"
  - "utm_source utm_medium"
  - "UTM cleanup"
  - "campaign tracking broken"
  - "channel grouping"
inputs:
  - current_utm_usage
  - channel_list
  - campaign_process
  - analytics_configuration
  - reporting_needs
outputs:
  - utm_taxonomy
  - naming_convention
  - automation_spec
  - cleanup_plan
  - reporting_rules
related_skills:
  - analytics-setup
  - attribution-model-selection
  - crm-pipeline-attribution
  - funnel-analysis
  - marketing-paid/paid-strategy
  - marketing-paid/performance-reporting
required_context:
  - .context/product-marketing.md
allowed_tools:
  - mcp:analytics
version: 1.0.0
---

## When to Use

Invoke when:
- Reports show forty spellings of "facebook" and nobody knows which is real
- Every channel manager invents their own campaign names
- Someone pastes a bare URL with no UTMs into a paid campaign
- Channel groupings in the analytics tool don't match how the business thinks
- Auditing why attribution numbers changed between quarters

## Workflow

### Step 1: Inventory the Current State
- Pull all distinct utm_source, utm_medium, and utm_campaign values from the analytics tool — the sprawl is the finding
- Count variants per intended value — facebook, Facebook, fb, facebook_ads, meta (case and naming drift)
- Identify missing UTMs — traffic landing with no source or medium at all, misattributed as direct
- Note who builds URLs today and how — spreadsheets, ad platform auto-tagging, manual pasting

**Gate:** Inventory of current values, variants, gaps, and URL-building processes written.

### Step 2: Define the Taxonomy
- Fix the parameter meanings first — source = where the traffic originates (facebook, google, newsletter), medium = the mechanism (cpc, email, social), campaign = the specific effort (spring_sale_us), content and term for the granular level
- Enforce lowercase, hyphens not underscores, no spaces; short, human-readable values
- Standardize a source/medium pair list per channel — one canonical pair each; new pairs require a decision, not improvisation
- Decide what not to tag — skip UTM on internal links and organic-to-organic navigation

**Gate:** Parameter definitions, canonical source/medium table, and formatting rules published.

### Step 3: Set the Naming Convention for Campaigns
- Build the convention around the dimensions your reporting groups by — e.g. channel, region, campaign, month as fixed segments (a convention decision, not a universal law — pick what your reports need)
- Make it sortable and predictable — consistent segment order, fixed segment count, no free text at the end
- Make it human-writable by non-engineers — if only one person can build a compliant URL, the convention will fail
- Document with examples and one worked case — one good example beats a page of rules

**Gate:** Campaign naming convention documented with examples and segment definitions.

### Step 4: Automate and Enforce
- Build a URL builder — a spreadsheet with data validation or a form — that emits compliant URLs (delegate automation to workflow-builder where it lives in the stack)
- Use ad platform auto-tagging where available (Google, Meta) and map its values to your taxonomy at the analytics layer instead of fighting it
- Validate at the source — form picklists, presets in scheduling tools, a browser extension for manual sharing
- Block the anti-pattern — bare URLs in campaign tools should be caught by review, not luck

**Gate:** URL builder live; auto-tagging mapped; the path of least resistance now produces compliant UTMs.

### Step 5: Fix Reporting Hygiene
- Rebuild channel groupings in the analytics tool to match the taxonomy — default groupings will not match your business
- Route unknown or missing values to a visible "Unassigned" bucket — never silently merged into direct
- Treat UTM data as capture-time truth — correcting history by editing links changes the past in analytics; re-tagging future campaigns is the fix
- Exclude internal and bot traffic from campaign reports

**Gate:** Channel groupings rebuilt; unassigned bucket visible; capture-time rule understood.

### Step 6: Clean Up, Monitor, and Own It
- Decide on legacy values — merge variants into canonical values at the analytics layer; do not edit historical links
- Set an owner — one person or team owns the taxonomy; changes go through them
- Review quarterly — new channels, new campaigns, new violations; publish the worst offenders to the reporting audience, not privately
- Re-run the Step 1 inventory as the audit — sprawl should shrink, not grow

**Gate:** Ownership assigned; quarterly audit scheduled; legacy cleanup plan executed or dated.

## Evaluation & QA

### Common Failure Modes
- A taxonomy so strict that people bypass it and tag nothing
- Auto-tagging and manual tagging mixed without a mapping — two overlapping source names forever
- Campaign names with the date in three different formats at the end
- Changing UTM conventions mid-year and blaming the attribution change on the market
- Cleaning history by renaming links, corrupting historical attribution
- No owner — governance documents everyone read once and nobody enforces
- UTM sprawl replicated in CRM source fields because form capture was never connected to the taxonomy
