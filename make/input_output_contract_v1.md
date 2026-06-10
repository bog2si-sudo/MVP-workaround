# Make.com Input / Output Contract v1

## Purpose

This file defines the exact data contract between Tally, Airtable, Make.com, OpenAI, PDF generation, Stripe, and email delivery.

The goal is to prevent vague automation logic and make every workflow testable.

---

# 1. Free Assessment Submission Contract

## Trigger

Tally webhook after the user completes Q1–Q15 and submits email.

## Required incoming fields from Tally

```json
{
  "email": "",
  "q1": "",
  "q2": "",
  "q3": "",
  "q4": "",
  "q5": "",
  "q6": "",
  "q7": "",
  "q8": "",
  "q9": "",
  "q10": "",
  "q11": "",
  "q12": "",
  "q13": "",
  "q14": "",
  "q15": "",
  "utm_source": "",
  "utm_medium": "",
  "utm_campaign": "",
  "referral_code": ""
}
```

## Airtable write: assessments

Create one assessment record with required fields:

```json
{
  "email": "",
  "q1": "",
  "q2": "",
  "q3": "",
  "q4": "",
  "q5": "",
  "q6": "",
  "q7": "",
  "q8": "",
  "q9": "",
  "q10": "",
  "q11": "",
  "q12": "",
  "q13": "",
  "q14": "",
  "q15": "",
  "stage": "",
  "industry": "",
  "region": "",
  "utm_source": "",
  "utm_medium": "",
  "utm_campaign": "",
  "referral_code": "",
  "lead_source": "free_assessment",
  "is_paid": false,
  "payment_status": "unpaid",
  "delivery_status": "not_started",
  "scoring_model_version": "v1",
  "prompt_version": "v1",
  "roadmap_version": "v1"
}
```

### Derived fields

- `stage` = `q13`
- `industry` = `q14`
- `region` = `q15`

---

# 2. Scoring Contract

## Input

Use Q1–Q12 only for score calculation.

Q13–Q15 are context fields and do not add score points.

## Output fields

Write these fields back to assessments:

```json
{
  "score_team": 0,
  "score_market": 0,
  "score_traction": 0,
  "score_product_econ": 0,
  "score_total": 0,
  "top_gap_1": "",
  "top_gap_2": "",
  "top_gap_3": "",
  "top_gaps": ""
}
```

## Gap ordering rule

Sort weakest categories by percentage of maximum score.

Category maximums:

```json
{
  "team": 20,
  "market": 25,
  "traction": 30,
  "product_econ": 25
}
```

Tie-break priority:

`traction > market > product_econ > team`

---

# 3. Free Result OpenAI Contract

## Trigger

After scoring is complete.

## Input sent to OpenAI

```json
{
  "assessment_type": "free",
  "email": "",
  "stage": "",
  "industry": "",
  "region": "",
  "answers": {
    "q1": "",
    "q2": "",
    "q3": "",
    "q4": "",
    "q5": "",
    "q6": "",
    "q7": "",
    "q8": "",
    "q9": "",
    "q10": "",
    "q11": "",
    "q12": "",
    "q13": "",
    "q14": "",
    "q15": ""
  },
  "scores": {
    "score_team": 0,
    "score_market": 0,
    "score_traction": 0,
    "score_product_econ": 0,
    "score_total": 0
  },
  "top_gaps": ["", "", ""],
  "benchmark_snapshot": {
    "summary": "",
    "confidence": "Low"
  },
  "prompt_version": "v1",
  "scoring_model_version": "v1"
}
```

## Required OpenAI response

OpenAI must return valid JSON:

```json
{
  "score_summary": "",
  "readiness_label": "",
  "top_gaps": [
    {
      "gap": "",
      "why_it_matters": "",
      "first_action": ""
    }
  ],
  "benchmark_summary": "",
  "upsell_copy": ""
}
```

## Airtable writeback

Write to assessments:

```json
{
  "summary": "{{score_summary}}",
  "benchmark_summary": "{{benchmark_summary}}",
  "top_gaps": "{{formatted_top_gaps}}"
}
```

## Event write

Create event:

```json
{
  "event_name": "assessment_completed",
  "email": "",
  "source": "make",
  "metadata": "free_result_generated"
}
```

---

# 4. Stripe Payment Contract

## Trigger

Stripe checkout/payment success webhook.

## Required incoming fields from Stripe

```json
{
  "stripe_payment_id": "",
  "email": "",
  "amount": 9,
  "currency": "USD",
  "status": "paid",
  "paid_at": ""
}
```

## Airtable write: payments

Create payment record:

```json
{
  "stripe_payment_id": "",
  "assessment_id": "",
  "email": "",
  "amount": 9,
  "currency": "USD",
  "status": "paid",
  "paid_at": ""
}
```

## Airtable update: assessments

Find assessment by email or assessment_id.

Update:

```json
{
  "is_paid": true,
  "payment_status": "paid"
}
```

## Event write

Create event:

```json
{
  "event_name": "roadmap_purchased",
  "email": "",
  "source": "stripe",
  "metadata": "payment_success"
}
```

---

# 5. Paid Add-On Contract

## Trigger

Paid add-on form submission or paid checkout completion.

## Required fields

```json
{
  "email": "",
  "q16": "",
  "q17": "",
  "q18": "",
  "q19": "",
  "q20": ""
}
```

## Airtable update: assessments

Find existing assessment by email.

Update:

```json
{
  "q16": "",
  "q17": "",
  "q18": "",
  "q19": "",
  "q20": ""
}
```

---

# 6. Paid Roadmap OpenAI Contract

## Trigger

Both conditions must be true:

- `is_paid` = `true`
- `q16-q20` are present

## Input sent to OpenAI

```json
{
  "assessment_type": "paid_roadmap",
  "email": "",
  "stage": "",
  "industry": "",
  "region": "",
  "answers": {
    "q1": "",
    "q2": "",
    "q3": "",
    "q4": "",
    "q5": "",
    "q6": "",
    "q7": "",
    "q8": "",
    "q9": "",
    "q10": "",
    "q11": "",
    "q12": "",
    "q13": "",
    "q14": "",
    "q15": "",
    "q16": "",
    "q17": "",
    "q18": "",
    "q19": "",
    "q20": ""
  },
  "scores": {
    "score_team": 0,
    "score_market": 0,
    "score_traction": 0,
    "score_product_econ": 0,
    "score_total": 0
  },
  "top_gaps": ["", "", ""],
  "benchmark_snapshot": {
    "summary": "",
    "confidence": "Low"
  },
  "prompt_version": "v1",
  "scoring_model_version": "v1",
  "roadmap_version": "v1"
}
```

## Required OpenAI response

OpenAI must return valid JSON:

```json
{
  "executive_summary": "",
  "score_and_gap_analysis": {
    "total_score": 0,
    "team": "",
    "market": "",
    "traction": "",
    "product_economics": ""
  },
  "top_3_priorities": [
    {
      "priority": "",
      "why_now": "",
      "success_metric": ""
    }
  ],
  "thirty_day_plan": [
    {
      "action": "",
      "owner": "",
      "success_metric": ""
    }
  ],
  "sixty_day_plan": [
    {
      "action": "",
      "owner": "",
      "success_metric": ""
    }
  ],
  "ninety_day_plan": [
    {
      "action": "",
      "owner": "",
      "success_metric": ""
    }
  ],
  "risks": [
    {
      "risk": "",
      "warning_signal": "",
      "mitigation": ""
    }
  ],
  "experiments": [
    {
      "experiment": "",
      "hypothesis": "",
      "how_to_run": "",
      "pass_fail_metric": ""
    }
  ],
  "one_page_memo": "",
  "consult_upsell": ""
}
```

## Airtable writeback

Write to assessments:

```json
{
  "roadmap_text": "{{full_formatted_roadmap}}",
  "priority_1": "{{priority_1}}",
  "priority_2": "{{priority_2}}",
  "priority_3": "{{priority_3}}",
  "risk_1": "{{risk_1}}",
  "risk_2": "{{risk_2}}",
  "risk_3": "{{risk_3}}",
  "experiment_1": "{{experiment_1}}",
  "experiment_2": "{{experiment_2}}",
  "experiment_3": "{{experiment_3}}",
  "investor_memo": "{{one_page_memo}}",
  "delivery_status": "generated"
}
```

## Event write

Create event:

```json
{
  "event_name": "roadmap_generated",
  "email": "",
  "source": "make",
  "metadata": "paid_roadmap_generated"
}
```

---

# 7. PDF Generation Contract

## Trigger

- `delivery_status` = `generated`
- `roadmap_text` is not empty
- `is_paid` = `true`

## Input to PDF provider

```json
{
  "email": "",
  "score_total": 0,
  "readiness_label": "",
  "executive_summary": "",
  "team_analysis": "",
  "market_analysis": "",
  "traction_analysis": "",
  "product_economics_analysis": "",
  "priority_1": "",
  "priority_2": "",
  "priority_3": "",
  "risk_1": "",
  "risk_2": "",
  "risk_3": "",
  "experiment_1": "",
  "experiment_2": "",
  "experiment_3": "",
  "benchmark_summary": "",
  "benchmark_confidence": "",
  "one_page_memo": ""
}
```

## PDF output

Expected response:

```json
{
  "pdf_link": ""
}
```

## Airtable writeback

```json
{
  "pdf_link": "",
  "delivery_status": "pdf_generated"
}
```

## Event write

```json
{
  "event_name": "pdf_generated",
  "email": "",
  "source": "pdf_provider",
  "metadata": "pdf_created"
}
```

---

# 8. Email Delivery Contract

## Trigger

- `delivery_status` = `pdf_generated`
- `pdf_link` is not empty

## Email input

```json
{
  "to": "",
  "subject": "Your Startup Readiness Roadmap is ready",
  "pdf_link": "",
  "score_total": 0,
  "top_gap_1": "",
  "top_gap_2": "",
  "top_gap_3": "",
  "consult_link": ""
}
```

## Airtable writeback

```json
{
  "delivery_status": "sent"
}
```

## Event write

```json
{
  "event_name": "roadmap_delivered",
  "email": "",
  "source": "email_provider",
  "metadata": "email_sent"
}
```

---

# 9. Consult Form Contract

## Trigger

Tally consult form submission.

## Required incoming fields

```json
{
  "email": "",
  "q21": "",
  "q22": "",
  "q23": "",
  "q24": "",
  "q25": "",
  "q26": "",
  "q27": "",
  "q28": "",
  "q29": "",
  "q30": ""
}
```

## Airtable write: consult_forms

```json
{
  "email": "",
  "q21": "",
  "q22": "",
  "q23": "",
  "q24": "",
  "q25": "",
  "q26": "",
  "q27": "",
  "q28": "",
  "q29": "",
  "q30": "",
  "status": "New"
}
```

## Event write

```json
{
  "event_name": "consult_clicked",
  "email": "",
  "source": "tally",
  "metadata": "consult_form_submitted"
}
```

---

# 10. Failure Handling

## Payment succeeded but no roadmap generated

**Action:**

- Set `delivery_status` = `failed`
- Create event: `roadmap_generation_failed`
- Notify operator manually

## OpenAI response is not valid JSON

**Action:**

- Retry once
- If second failure occurs, set `delivery_status` = `failed`
- Store raw response in metadata or notes
- Notify operator manually

## PDF generation fails

**Action:**

- Retry once
- If second failure occurs, set `delivery_status` = `failed`
- Notify operator manually

## Email delivery fails

**Action:**

- Retry once
- If second failure occurs, set `delivery_status` = `failed`
- Notify operator manually

---

# 11. Non-Negotiable Rules

- Do not generate a paid roadmap unless payment is confirmed.
- Do not overwrite raw assessment answers.
- Do not invent benchmark data.
- Do not hide low benchmark confidence.
- Do not describe the roadmap as investment advice.
- Do not send a delivery email without a working PDF link.
- Always store `prompt_version`, `scoring_model_version`, and `roadmap_version`.
