# Airtable Schema v1

## Purpose

This file defines the Airtable structure for the Startup Readiness MVP.

The goal is to keep the MVP simple, traceable, and automation-ready.

## Base

Recommended base name:

Startup Readiness MVP

## Table 1: assessments

Stores assessment answers, scores, generated roadmap content, PDF links, and tracking metadata.

### Primary field

| Field name | Type | Notes |
|---|---|---|
| email | Email | Primary identifier for MVP |

### Core founder context

| Field name | Type | Notes |
|---|---|---|
| stage | Single line text | Pre-seed, Seed, Early growth, Later growth |
| industry | Single line text | SaaS, E-commerce, Marketplace, Consumer app, Services, Other |
| region | Single line text | EU, US, Other |

### Free assessment answers

| Field name | Type | Notes |
|---|---|---|
| q1 | Single line text | Built something before? |
| q2 | Single line text | Relevant co-founder/core team experience? |
| q3 | Single line text | Execution ability next 6 months |
| q4 | Single line text | Market size |
| q5 | Single line text | Problem urgency |
| q6 | Single line text | Customer clarity |
| q7 | Single line text | Paying customers |
| q8 | Single line text | Monthly revenue |
| q9 | Single line text | Revenue repeatability |
| q10 | Single line text | Differentiation |
| q11 | Single line text | Unit economics |
| q12 | Single line text | Distribution strength |
| q13 | Single line text | Current stage |
| q14 | Single line text | Industry |
| q15 | Single line text | Region |

### Paid add-on answers

| Field name | Type | Notes |
|---|---|---|
| q16 | Single line text | Biggest bottleneck |
| q17 | Single line text | 6-month goal |
| q18 | Single line text | Weekly growth time |
| q19 | Single line text | Growth/marketing budget |
| q20 | Single line text | Investor/advisor access |

### Scores

| Field name | Type | Notes |
|---|---|---|
| score_total | Number | 0–100 |
| score_team | Number | 0–20 |
| score_market | Number | 0–25 |
| score_traction | Number | 0–30 |
| score_product_econ | Number | 0–25 |

### Gap outputs

| Field name | Type | Notes |
|---|---|---|
| top_gaps | Long text | Combined top gaps summary |
| top_gap_1 | Single line text | Highest-priority gap |
| top_gap_2 | Single line text | Second-priority gap |
| top_gap_3 | Single line text | Third-priority gap |
| summary | Long text | Free result summary |
| benchmark_summary | Long text | Benchmark snapshot shown to user |
| benchmark_confidence | Single line text | High, Medium, Low |

### Paid roadmap outputs

| Field name | Type | Notes |
|---|---|---|
| roadmap_text | Long text | Full generated roadmap |
| priority_1 | Long text | First roadmap priority |
| priority_2 | Long text | Second roadmap priority |
| priority_3 | Long text | Third roadmap priority |
| risk_1 | Long text | First risk |
| risk_2 | Long text | Second risk |
| risk_3 | Long text | Third risk |
| experiment_1 | Long text | First experiment |
| experiment_2 | Long text | Second experiment |
| experiment_3 | Long text | Third experiment |
| investor_memo | Long text | One-page memo |
| pdf_link | URL | Generated PDF link |

### Payment and delivery state

| Field name | Type | Notes |
|---|---|---|
| is_paid | Checkbox | True after successful Stripe payment |
| payment_status | Single line text | unpaid, paid, refunded, failed |
| delivery_status | Single line text | not_started, generated, sent, failed |

### Tracking and versions

| Field name | Type | Notes |
|---|---|---|
| prompt_version | Single line text | Example: v1 |
| roadmap_version | Single line text | Example: v1 |
| scoring_model_version | Single line text | Example: v1 |
| lead_source | Single line text | Manual source label |
| utm_source | Single line text | Optional |
| utm_medium | Single line text | Optional |
| utm_campaign | Single line text | Optional |
| referral_code | Single line text | Optional |
| created_at | Date/time | Europe/Bucharest timezone recommended |

---

## Table 2: payments

Stores Stripe payment records.

### Primary field

| Field name | Type | Notes |
|---|---|---|
| stripe_payment_id | Single line text | Stripe payment/session ID |

### Fields

| Field name | Type | Notes |
|---|---|---|
| assessment_id | Single line text | MVP link to assessment; can become linked record later |
| email | Email | Buyer email |
| amount | Currency | Usually 9 |
| currency | Single line text | USD, EUR, etc. |
| status | Single select | pending, paid, failed, refunded |
| paid_at | Date/time | When payment succeeded |

---

## Table 3: benchmarks_static

Stores simple benchmark rows.

Important: in v1, benchmarks may be internal/synthetic baselines. If so, label them clearly.

### Primary field

| Field name | Type | Notes |
|---|---|---|
| stage | Single line text | Pre-seed, Seed, Early growth, Later growth |

### Fields

| Field name | Type | Notes |
|---|---|---|
| industry | Single line text | SaaS, Marketplace, etc. |
| region | Single line text | EU, US, Other |
| metric_name | Single line text | Example: monthly revenue |
| metric_value | Number | Numeric benchmark value |
| source | Single line text | Source name or MVP synthetic baseline |
| confidence | Single line text | High, Medium, Low |
| notes | Long text | Explanation or caveat |

---

## Table 4: consult_forms

Stores premium consult intake responses.

### Primary field

| Field name | Type | Notes |
|---|---|---|
| email | Email | Founder email |

### Fields

| Field name | Type | Notes |
|---|---|---|
| q21 | Long text | Primary customer segments |
| q22 | Long text | Customer acquisition attempts |
| q23 | Long text | Biggest failure and lesson |
| q24 | Long text | Fundraising target/timeline |
| q25 | Long text | Competitors and edge |
| q26 | Long text | Team structure |
| q27 | Long text | Hiring plan |
| q28 | Long text | Funnel metrics |
| q29 | Long text | Risks and constraints |
| q30 | Long text | Help wanted most |
| help_needed | Long text | Condensed need |
| notes | Long text | Internal notes |
| status | Single select | New, Reviewed, Booked, Completed, No fit |
| booking_link | URL | Calendly or booking link |
| booked_call_at | Date/time | Scheduled call time |

---

## Table 5: events

Stores basic funnel analytics.

### Primary field

| Field name | Type | Notes |
|---|---|---|
| event_name | Single line text | Event type |

### Fields

| Field name | Type | Notes |
|---|---|---|
| email | Email | User email, if known |
| assessment_id | Single line text | Optional |
| source | Single line text | Origin system |
| metadata | Long text | JSON or plain notes |
| created_at | Date/time | Event timestamp |

### Event names

Recommended values:

assessment_started  
assessment_completed  
email_captured  
roadmap_purchased  
roadmap_generated  
pdf_generated  
roadmap_delivered  
pdf_downloaded  
consult_clicked  
consult_booked  

---

## Table 6: prompt_versions

Stores prompts used by the automation.

### Primary field

| Field name | Type | Notes |
|---|---|---|
| version | Single line text | Example: v1 |

### Fields

| Field name | Type | Notes |
|---|---|---|
| prompt_type | Single select | free_result, paid_roadmap, consult_analysis, scoring |
| prompt_text | Long text | Full prompt |
| active | Checkbox | True if currently used |
| created_at | Date/time | Version creation date |

---

## Table relationships

For MVP speed, use plain text IDs first.

Later upgrade path:

- payments.assessment_id to linked record in assessments
- events.assessment_id to linked record in assessments
- consult_forms.email to lookup or linked record in assessments

## Data-quality rules

1. Never overwrite raw answers.
2. Store generated output separately from user input.
3. Always store prompt version and scoring model version.
4. Label synthetic benchmarks as low confidence.
5. Preserve payment state separately from roadmap generation state.
