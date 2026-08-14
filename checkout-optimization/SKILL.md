---
name: checkout-optimization
category: cro
description: Improve checkout conversion — payment friction, form and address fields, shipping thresholds, trust signals, express and guest checkout, cart recovery.
triggers:
  - "checkout conversion"
  - "cart abandonment"
  - "payment method friction"
  - "shipping threshold"
  - "guest checkout"
  - "express checkout"
  - "checkout drop-off"
inputs:
  - checkout_url
  - funnel_data
  - payment_methods
  - shipping_options
  - cart_abandonment_rate
outputs:
  - friction_map
  - checkout_optimization_plan
  - trust_signal_inventory
  - cart_recovery_plan
related_skills:
  - cro-audit
  - funnel-analysis
  - forms-microcopy
  - ab-testing
  - experiment-prioritization
  - analytics-setup
  - marketing-channels/lifecycle-sequences
  - marketing-messaging/objection-handling
  - marketing-paid/shopify-marketing-audit
required_context:
  - .context/product-marketing.md
allowed_tools:
  - mcp:analytics
  - mcp:heatmap
version: 1.0.0
---

## When to Use

Invoke when:
- Checkout funnel shows a steep drop between cart and purchase
- Cart abandonment rate is high and nobody has quantified where users leave
- Adding or removing payment methods, or a provider change, is on the roadmap
- Shipping cost surprises are suspected of killing conversions
- Evaluating express checkout options (Shop Pay, Apple Pay, PayPal, Amazon Pay, one-click)
- Planning a cart abandonment email or SMS recovery flow

## Workflow

### Step 1: Quantify the Checkout Funnel
- Instrument every step with unique events — cart view, checkout start, shipping step, payment step, review, purchase (delegate event taxonomy to analytics-setup)
- Calculate step-to-step conversion and where revenue actually leaks
- Segment by device, traffic source, new vs returning, and order value bucket
- Define guardrail metrics before changing anything — revenue per visitor, average order value, cancellation rate, support contacts
- Heuristic: treat any step below roughly 80% step conversion as a candidate for audit (labeled heuristic — verify against your own baseline)

**Gate:** Checkout funnel mapped with step conversion rates, two worst steps identified, guardrails defined.

### Step 2: Audit Payment Method Friction
- List every payment method offered, with the cost and risk of each
- Check for forced account creation before payment — a classic killer
- Verify how payment errors (declines, timeouts) are communicated to the user
- Check localization — currency, language, address formats, and local payment methods for non-domestic traffic
- Match methods to actual customer geography and behavior, not to competitor feature lists (assumption to verify with session data)

**Gate:** Payment friction list written — each item mapped to evidence and a proposed fix.

### Step 3: Streamline Forms and Address Entry
- Collect only fields needed to fulfill the order — drop company name, fax, and phone unless required
- Enable address autocomplete — reduces typos and keystrokes on the longest field set in the flow
- Use smart defaults — billing same as shipping, country pre-set from IP
- Delegate field-level rules — selection, validation, error copy — to forms-microcopy

**Gate:** Field list minimized and justified; autocomplete and smart defaults specified.

### Step 4: Fix Shipping Thresholds and Cost Transparency
- Show shipping cost as early as possible — surprise shipping at the payment step is a top abandonment driver (heuristic, consistent across published CRO research — label it)
- If you use a free-shipping threshold, display progress toward it in the cart ("Add $12 for free shipping")
- Test threshold level against margin, not against conversion alone — a threshold that lifts conversion can destroy unit economics
- Treat flat rate vs free vs carrier-calculated as a test, not a belief

**Gate:** Shipping cost visibility plan written; threshold economics modeled with margins.

### Step 5: Add Trust Signals at the Point of Risk
- Place trust elements where anxiety peaks — the payment step, not just the homepage
- Show real, specific reassurance — accepted payment logos, SSL badge, return policy one-liner, support contact, order preview
- Never fabricate reviews, counts, or "as seen in" logos — fabricated trust signals are a fraud risk, not a CRO trick
- Match the signal to the doubt — price-sensitive traffic needs policy clarity; new visitors need legitimacy

**Gate:** Trust signal inventory placed against each high-anxiety step; every claim verifiable.

### Step 6: Decide on Express Checkout and Guest Checkout
- Default to guest checkout; offer account creation after the order, not before
- Use express checkout (wallet buttons — Apple Pay, Shop Pay, PayPal, Amazon Pay) where typing is painful — mobile, repeat ecosystems
- Reserve one-click for repeat buyers — express buttons on a first visit add visual noise without a stored credential (assumption — test)
- Keep express and full checkout in sync on discounts, taxes, and shipping — a mismatch silently breaks both flows

**Gate:** Guest-vs-account and express-vs-standard decisions made per device and audience, with rollback plan.

### Step 7: Build Cart Abandonment Recovery
- Trigger an email or SMS sequence — timing heuristic: first touch within an hour, follow-ups at 24 hours and 3 days (labeled heuristic; test your own cadence)
- Include cart contents and a direct return link that restores the session
- Segment messages by likely abandonment reason where known — price, shipping, just browsing
- Add retargeting only after owned-channel recovery; avoid bombarding recovered buyers
- Measure recovery revenue net of sending cost, not gross sends

**Gate:** Recovery sequence drafted with timing, content, and net-revenue measurement defined.

### Step 8: Test and Verify
- One change per test, proper sample size — delegate to ab-testing
- Prioritize by expected impact and effort — delegate to experiment-prioritization
- Check guardrails after every change — conversion up but AOV or margin down is a loss
- Re-baseline the funnel after each shipped change so the next audit starts from truth

**Gate:** Every change shipped with a test or documented reason; guardrails checked; funnel re-baselined.

## Practitioner Grounding
- **Baymard Institute** (Christian Holst) — the primary checkout-research authority: 14 years, 54,000+ hours, 272 think-aloud sessions, 11,777 survey participants, 150+ site audits. Checkout design is frequently the SOLE cause of abandonment for cart-adders (EMPIRICAL, T1).
- **Baymard consumer research (2024)** — 48% of abandoners cite extra costs higher than expected (#1); 26% cite forced account creation (#2); ~42% were "browsing, not ready to buy" (unavoidable); avg cart abandonment 70.2% (EMPIRICAL, T1).
- **Baymard form-field benchmarks** — average checkout shows 23.48 form elements vs ~12 achievable (7 fields); 20–60% reduction possible; 16→8 fields ≈ 25–35% conversion lift; perceived field count beats actual: 15 fields across 3 logical steps outperforms 10 fields on one page by 11–14% (EMPIRICAL, T1).
- **Baymard A/B evidence** — one-page vs multi-step showed no significant difference; format is a context decision (AOV, complexity, mobile share), not dogma (EMPIRICAL, T1).
- **Peep Laja / Jon MacDonald** — CRO process (research first, guardrails, traffic thresholds) applies to checkout tests (FRAMEWORK, T2).

## Decision Rules
1. IF shipping/taxes/fees are not shown before the payment step THEN surface them earlier — top abandonment cause at 48% (Baymard, EMPIRICAL, T1).
2. IF checkout requires account creation THEN make guest checkout the default; offer account creation after purchase (Baymard delayed-account-creation, EMPIRICAL, T1).
3. IF default form elements >12 THEN cut 20–60% before testing anything else (Baymard, EMPIRICAL, T1).
4. IF choosing flow format THEN one-page/accordion for AOV <$150, simple products, mobile-heavy traffic; multi-step for AOV >$200, B2B/configurable, extra info collection (Baymard, EMPIRICAL, T1).
5. IF a long form must stay long THEN group it into 3 logical steps — perceived simplicity wins by 11–14% (Baymard, EMPIRICAL, T1).
6. IF abandonment data says "browsing" THEN exclude that share from optimization targets — it's not fixable in checkout (Baymard, EMPIRICAL, T1).
7. IF a checkout change is tested AND traffic is low THEN use the low-traffic path (big-swing test or ship + honest before/after), not a long small-effect A/B (MacDonald, HEURISTIC, T2).

## Metrics
- Primary: cart→purchase conversion; step conversion per checkout stage.
- Guardrails: revenue per visitor, AOV, margin per order, cancellation/refund rate, support contacts (a conversion win that destroys margin is a loss).
- Diagnostic: abandonment reason mix (costs vs account vs complexity vs browsing) via exit surveys/analytics; default form-element count vs 12; mobile vs desktop step conversion.
- Timebox: re-measure 2 weeks after each shipped change; re-baseline the funnel quarterly.

## Practitioner Failure Modes
- Chasing the ~42% "browsing" share — unfixable in checkout (Baymard).
- Hiding shipping cost until the payment step (Baymard #1 cause).
- Forcing account creation pre-purchase (Baymard #2 cause).
- Aggressive validation rejecting legitimate input — users leave when they can't complete a field (Baymard).
- Cutting fields fulfillment actually needs — cut with a consumer, not a scalpel (Baymard).
- Adding payment methods "because competitors have them" with no customer evidence (skill-consistent).
- Shipping threshold that lifts conversion but loses money on margin (skill-consistent).

## Sources
1. Baymard Institute — Checkout Usability research overview | baymard.com/research/checkout-usability | T1 | 2026-08-15
2. Christian Holst — Reasons for Cart Abandonment – Why 70% of Do So | baymard.com/blog/ecommerce-checkout-usability-report-and-benchmark | T1 | 2026-08-15
3. Baymard — Checkout flow average form fields | baymard.com/blog/checkout-flow-average-form-fields | T1 | 2026-08-15
4. Baymard — Checkout optimization from 16 fields to 8 | baymard.com/blog/checkout-optimization-from-16-fields-to-8 | T1 | 2026-08-15
5. Baymard — Cart abandonment rate list | baymard.com/lists/cart-abandonment-rate | T1 | 2026-08-15
6. Panel: practitioner-intelligence/domains/optimize-longtail/baymard.md | T1/T2 | 2026-08-15

## Evaluation & QA

### Common Failure Modes
- Optimizing checkout but never measuring beyond it — cart-to-purchase up, total revenue flat
- Adding six payment methods "because competitors have them" with no customer evidence
- Shipping threshold that lifts conversion but loses money on margin
- Generic trust badges nobody reads, or fabricated claims
- Express checkout buttons fighting the standard flow over discounts and taxes
- Cart recovery emails that link to an empty cart
- Fixing checkout friction by adding fields "for marketing data"
