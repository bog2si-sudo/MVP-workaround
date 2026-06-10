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
