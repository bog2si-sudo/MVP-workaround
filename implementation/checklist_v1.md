# Implementation Checklist v1

## Purpose

This checklist defines the build order for the Startup Readiness MVP.

The goal is to launch a working funnel before optimizing design, benchmarks, or automation complexity.

## Phase 1: Airtable foundation

Status target: database ready for form submissions and automation.

### Tasks

- [ ] Create Airtable base: Startup Readiness MVP
- [ ] Create table: assessments
- [ ] Add Q1–Q20 fields to assessments
- [ ] Add score fields to assessments
- [ ] Add roadmap output fields to assessments
- [ ] Add metadata/version fields to assessments
- [ ] Create table: payments
- [ ] Create table: benchmarks_static
- [ ] Create table: consult_forms
- [ ] Create table: events
- [ ] Create table: prompt_versions
- [ ] Add initial prompt version rows
- [ ] Add initial benchmark rows with confidence labels

### Completion criteria

- Airtable can store one full free assessment.
- Airtable can store one full paid assessment.
- Airtable can store generated roadmap content.
- Airtable can store payment and event records.

---

## Phase 2: Tally forms

Status target: users can submit assessment data.

### Forms

Create three Tally forms:

1. Free assessment form
2. Paid add-on form
3. Consult pre-call form

### Free assessment form

- [ ] Add Q1–Q15
- [ ] Add email capture after Q15
- [ ] Add hidden fields for UTM data:
  - utm_source
  - utm_medium
  - utm_campaign
  - referral_code
- [ ] Connect form submission to Make.com webhook
- [ ] Test one full submission

### Paid add-on form

- [ ] Add Q16–Q20
- [ ] Capture email
- [ ] Connect to existing assessment through email or assessment ID
- [ ] Connect form submission to Make.com webhook

### Consult form

- [ ] Add Q21–Q30
- [ ] Capture email
- [ ] Add preferred contact method
- [ ] Add optional Calendly redirect or link
- [ ] Connect to Make.com webhook

### Completion criteria

- Free form creates an Airtable assessment record.
- Paid form updates or enriches the same assessment.
- Consult form creates a consult record.

---

## Phase 3: Scoring logic

Status target: every completed assessment receives a score.

### Tasks

- [ ] Implement scoring model v1 in Make.com or Airtable formula fields
- [ ] Compute:
  - score_team
  - score_market
  - score_traction
  - score_product_econ
  - score_total
- [ ] Determine:
  - top_gap_1
  - top_gap_2
  - top_gap_3
- [ ] Store scoring_model_version = v1
- [ ] Test with at least five fake founder profiles

### Test profiles

- [ ] First-time founder, no traction
- [ ] Strong team, weak market
- [ ] Weak team, strong traction
- [ ] Strong market, weak distribution
- [ ] High-revenue founder with poor repeatability

### Completion criteria

- Scores are consistent.
- Top gaps make practical sense.
- Score bands match the scoring model.

---

## Phase 4: Free result generation

Status target: free users receive useful diagnostic output.

### Tasks

- [ ] Add free assessment prompt to prompt_versions
- [ ] Connect Make.com to OpenAI
- [ ] Send assessment data and scores to OpenAI
- [ ] Require JSON output
- [ ] Parse JSON response
- [ ] Save:
  - summary
  - top_gaps
  - benchmark_summary
- [ ] Create assessment_completed event
- [ ] Create email_captured event
- [ ] Send or display free result

### Completion criteria

- Free result is generated from real submitted answers.
- Output is clear and not generic.
- User sees a clear $9 roadmap upsell.

---

## Phase 5: Stripe payment

Status target: user can pay for the roadmap.

### Tasks

- [ ] Create Stripe product: Startup Readiness Roadmap
- [ ] Set price: $9
- [ ] Create payment link or checkout session
- [ ] Add success URL
- [ ] Add cancel URL
- [ ] Configure Stripe webhook to Make.com
- [ ] On successful payment:
  - Create payment record
  - Update assessments.is_paid = true
  - Update payment_status = paid
  - Create roadmap_purchased event

### Completion criteria

- Test payment updates Airtable.
- Failed/cancelled payment does not generate roadmap.
- Successful payment triggers roadmap generation.

---

## Phase 6: Paid roadmap generation

Status target: paid users receive a personalized roadmap.

### Tasks

- [ ] Add paid roadmap prompt to prompt_versions
- [ ] Retrieve full Q1–Q20 answers
- [ ] Retrieve scores and top gaps
- [ ] Retrieve benchmark snapshot
- [ ] Send structured input to OpenAI
- [ ] Parse JSON response
- [ ] Save:
  - roadmap_text
  - priority_1
  - priority_2
  - priority_3
  - risk_1
  - risk_2
  - risk_3
  - experiment_1
  - experiment_2
  - experiment_3
  - investor_memo
- [ ] Store prompt_version = v1
- [ ] Store roadmap_version = v1
- [ ] Create roadmap_generated event

### Completion criteria

- Roadmap contains specific 30/60/90-day actions.
- Roadmap reflects the user's bottleneck and 6-month goal.
- Missing data is called out instead of invented.

---

## Phase 7: PDF generation

Status target: roadmap can be downloaded as a document.

### Tasks

- [ ] Choose PDF provider:
  - Documint
  - PDF.co
  - Google Docs template export
- [ ] Add roadmap template
- [ ] Map Airtable fields to PDF variables
- [ ] Generate PDF after roadmap text exists
- [ ] Save PDF URL to pdf_link
- [ ] Update delivery_status = generated
- [ ] Create pdf_generated event

### Completion criteria

- PDF link opens correctly.
- PDF includes all major roadmap sections.
- PDF contains disclaimer.

---

## Phase 8: Email delivery

Status target: paid user receives roadmap automatically.

### Tasks

- [ ] Choose email provider:
  - Resend
  - Brevo
- [ ] Create roadmap delivery email template
- [ ] Insert:
  - PDF link
  - Score
  - Top gaps
  - Consult link
- [ ] Send email after PDF generation
- [ ] Update delivery_status = sent
- [ ] Create roadmap_delivered event

### Completion criteria

- Email arrives in inbox.
- PDF link works.
- Consult upsell is visible but not aggressive.

---

## Phase 9: Consult upsell

Status target: users can book or request premium help.

### Tasks

- [ ] Create consult page or Calendly link
- [ ] Add consult CTA to:
  - Free result
  - Paid roadmap PDF
  - Delivery email
- [ ] Create consult intake form Q21–Q30
- [ ] Store consult form in consult_forms
- [ ] Create consult_clicked event
- [ ] Create consult_booked event when booked

### Completion criteria

- A user can request or book a consult.
- Consult record contains enough context for a human review.

---

## Phase 10: Analytics and review

Status target: MVP has enough signal to decide what to improve.

### Events to track

- [ ] assessment_started
- [ ] assessment_completed
- [ ] email_captured
- [ ] roadmap_purchased
- [ ] roadmap_generated
- [ ] pdf_generated
- [ ] roadmap_delivered
- [ ] pdf_downloaded
- [ ] consult_clicked
- [ ] consult_booked

### Weekly review metrics

- [ ] Assessment starts
- [ ] Assessment completions
- [ ] Email capture rate
- [ ] Paid conversion rate
- [ ] Roadmap delivery success rate
- [ ] Consult click rate
- [ ] Consult booking rate
- [ ] Refunds or complaints
- [ ] Manual fixes required

### Completion criteria

- Funnel weak point is visible.
- Manual failures are logged.
- Next improvement can be decided from data.

---

## Launch checklist

Before public launch:

- [ ] Test free assessment end-to-end
- [ ] Test paid roadmap end-to-end
- [ ] Test failed payment path
- [ ] Test PDF generation
- [ ] Test email delivery
- [ ] Test consult form
- [ ] Confirm disclaimers are visible
- [ ] Confirm no generated output claims to be valuation, investment advice, legal advice, or tax advice
- [ ] Confirm benchmark confidence is shown when benchmark is low confidence

## MVP success criteria

The MVP is successful if, within the first test cohort:

- At least 30 founders complete the free assessment.
- At least 3 users buy the $9 roadmap.
- At least 1 user requests or books a consult.
- At least 70% of generated roadmaps require no manual correction.
- No user reports that the output is misleading or falsely precise.

## Stop conditions

Pause automation and review if:

- Roadmaps contain invented metrics.
- Payment succeeds but roadmap is not delivered.
- PDF links break.
- Users misunderstand the product as investment advice.
- Benchmarks are presented without confidence labels.
