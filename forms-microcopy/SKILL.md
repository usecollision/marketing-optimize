---
name: forms-microcopy
category: cro
description: Design low-friction forms with sharp microcopy — field reduction, validation rules, error and button copy, placeholders, progressive disclosure.
triggers:
  - "form design"
  - "microcopy"
  - "form field reduction"
  - "error message copy"
  - "button label"
  - "placeholder text"
  - "progressive disclosure"
inputs:
  - form_url_or_spec
  - form_purpose
  - current_fields
  - validation_rules
  - conversion_data
outputs:
  - field_inventory
  - revised_field_list
  - error_copy_spec
  - button_copy_spec
related_skills:
  - cro-audit
  - landing-page-optimization
  - signup-flow
  - checkout-optimization
  - ab-testing
  - marketing-messaging/conversion-copywriting
  - marketing-messaging/customer-language-bank
required_context:
  - .context/product-marketing.md
allowed_tools: none
version: 1.0.0
---

## When to Use

Invoke when:
- A form exists and its completion rate is below target
- Someone wants to add a field "just in case" or "for marketing"
- Error messages are developer-written ("Invalid input") and unhelpful
- Button labels are generic ("Submit") and untested
- A long form needs restructuring without losing data quality

## Workflow

### Step 1: Inventory Every Field and Its Job
- List every field with: why it exists, who consumes it, can it be derived, does the user have the answer handy
- Classify — required for the transaction, required by law, marketing-wants, nice-to-have
- Kill or defer fields with no current consumer; move marketing data collection to post-conversion
- Heuristic: every field costs measurable completion rate — assume each unnecessary field costs a few percent and verify with your own tests (labeled heuristic)

**Gate:** Field inventory written; every field has a named owner and a stated reason, or is cut.

### Step 2: Reduce and Reorder
- One column, top-aligned labels, no inline placeholders used as labels (accessibility and clarity)
- Ask the hardest questions last; group related fields; use one question per screen when the form is high-stakes
- Pre-fill from known context — email to name, IP to country, referrer to interest
- Use the right input type — numeric keypad for digits, date picker for dates, autocomplete attributes for address and payment fields

**Gate:** Field list reduced to essentials, ordered ease-first, with pre-fill opportunities listed.

### Step 3: Write Labels and Placeholders
- Label states what to enter ("Email address"); placeholder shows an example, never a duplicate of the label ("name@company.com")
- Use the customer's own vocabulary — delegate to marketing-messaging/customer-language-bank
- Spell out why when data is sensitive — "We need your phone to send the delivery PIN" (pattern, not law — test against your audience)
- Never rely on placeholders alone — labels must persist while the user types

**Gate:** Label and placeholder spec written in customer language, with reasons for sensitive fields.

### Step 4: Define Validation and Error Copy
- Validate on blur or on submit, not on every keystroke
- Error messages say what happened, why it matters, and how to fix it — "This card number looks short — check for missing digits," not "Invalid input"
- Put the error next to the field, not in a distant banner; preserve the value the user typed
- Forgive formatting — accept spaces in card numbers, lowercase emails, dashes in phone numbers
- Reserve blocking errors for real blockers; warn instead of block for soft problems (weak password)

**Gate:** Every validation rule has user-facing copy with cause and fix, placed inline.

### Step 5: Write the Button Copy
- Describe the outcome, not the mechanism — "Start free trial" over "Submit"; "Get my report" over "Click here"
- Match the promise made before the form — button copy that matches the ad or email converts better (assumption — test)
- State the cost after the action when relevant — "Create account — free, no card required"
- Keep secondary actions quieter in both text and weight ("Back," not a competing button)

**Gate:** Primary and secondary button copy written to match the pre-form promise.

### Step 6: Apply Progressive Disclosure
- Show only what this step needs — shipping fields after billing choice, team size after plan choice
- Use conditional logic to skip irrelevant branches entirely
- Use a review step for high-stakes submissions instead of repeating fields
- Never hide required fields behind expanders users will not open

**Gate:** Conditional branches mapped; every hidden field reachable only when needed.

### Step 7: QA, Accessibility, and Test
- Tab through the form with a screen reader; confirm every error is announced and linked to its field
- Test the full keyboard path — no field traps, no lost focus
- Check every state — empty submit, wrong format, pasted text, autofill interference, slow network
- A/B the big copy decisions — label phrasing, button text, error wording — delegate to ab-testing

**Gate:** Accessibility pass done, edge states tested, test plan for copy variants written.

## Practitioner Grounding
- **Baymard Institute** — form-field research: average checkout has 23.48 form elements vs ~12 achievable; 20–60% reduction possible; 16→8 fields ≈ 25–35% conversion lift; users abandon when they can't complete a field (EMPIRICAL, T1).
- **Peep Laja** — friction-first CRO: every field is a gate; research before hypothesis; copy from customer research (FRAMEWORK, T2).
- **Michael Aagaard** — microcopy voice: outcome-driven button copy ("Get my …" patterns replicated across sites); trust/privacy microcopy can hurt when added without testing (−18.7% case) (EMPIRICAL, T2).
- **Jon MacDonald** — traffic thresholds: <1K visits/week, no valid small-effect A/B; run big-swing copy tests or ship reversibly (HEURISTIC, T2).
- **NN/g** — labels persist, placeholders exemplify; error messages must be recoverable (FRAMEWORK, T2).

## Decision Rules
1. IF a field has no current consumer THEN kill it or defer to post-conversion — field reduction is the highest-leverage form change (Baymard/Laja, EMPIRICAL, T2).
2. IF a form has >12 default elements THEN plan a 20–60% reduction before testing copy (Baymard, EMPIRICAL, T1).
3. IF an error message lacks cause + fix THEN rewrite inline next to the field, preserving the user's input (NN/g/Baymard, FRAMEWORK, T2).
4. IF button copy describes the mechanism ("Submit") THEN change it to the outcome promised before the form (Aagaard, EMPIRICAL, T2).
5. IF validation rejects formats users naturally type THEN forgive (spaces, dashes, lowercase) — aggressive validation drives abandonment (Baymard, EMPIRICAL, T1).
6. IF microcopy tests are planned on a <1K visits/week form THEN run one big copy swing or ship + honest before/after, not a long A/B (MacDonald, HEURISTIC, T2).
7. IF adding a trust/privacy element THEN test it — trust additions can reduce conversion (Aagaard, EMPIRICAL, T2).

## Metrics
- Primary: form completion rate; per-field abandonment (where in the form users leave).
- Secondary: error rate per field, re-submission rate after error, data quality retained (completeness of collected fields).
- Guardrails: valid-lead rate (don't trade quality for completion), support contacts about forms, time-to-complete.
- Timebox: re-measure 2 weeks after a shipped change; re-audit fields quarterly against consumers.

## Practitioner Failure Modes
- Placeholder-as-label forms that become illegible once typing starts (NN/g).
- Error messages that say what went wrong but not how to fix it (NN/g/Baymard).
- Adding fields back "for segmentation" after completion rates dropped (Laja).
- Progressive disclosure hiding required fields users never open (Baymard).
- Aggressive validation rejecting legitimate input (Baymard, EMPIRICAL).
- Polishing microcopy while the real problem is a broken submit handler (skill-consistent).
- Trust-element additions without testing (Aagaard privacy-policy case).

## Sources
1. Baymard Institute — Checkout flow average form fields | baymard.com/blog/checkout-flow-average-form-fields | T1 | 2026-08-15
2. Baymard — Checkout optimization from 16 fields to 8 | baymard.com/blog/checkout-optimization-from-16-fields-to-8 | T1 | 2026-08-15
3. Michael Aagaard — copy/microcopy case studies | aagaard.co / CXL | T2 | 2026-08-15
4. Peep Laja — ResearchXL friction research | conversionxl.com | T2 | 2026-08-15
5. NN/g — form design & error message guidelines | nngroup.com | T2 | 2026-08-15
6. Jon MacDonald — traffic thresholds (The Good) | thegood.com | T2 | 2026-08-15
7. Synthesis: practitioner-intelligence/syntheses/cro.md + domains/optimize-longtail/baymard.md | T1/T2 | 2026-08-15

## Evaluation & QA

### Common Failure Modes
- Placeholder-as-label forms that become illegible once typing starts
- Error messages that state what went wrong but not how to fix it
- Button copy that promises what the next page contradicts
- Adding fields back "for segmentation" after completion rates dropped
- Progressive disclosure that hides required steps from users who never open them
- Validation so aggressive that legitimate input (formats, prefixes, spaces) is rejected
- Polishing microcopy while the real problem is a broken submit handler
