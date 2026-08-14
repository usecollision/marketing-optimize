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

## Evaluation & QA

### Common Failure Modes
- Placeholder-as-label forms that become illegible once typing starts
- Error messages that state what went wrong but not how to fix it
- Button copy that promises what the next page contradicts
- Adding fields back "for segmentation" after completion rates dropped
- Progressive disclosure that hides required steps from users who never open them
- Validation so aggressive that legitimate input (formats, prefixes, spaces) is rejected
- Polishing microcopy while the real problem is a broken submit handler
