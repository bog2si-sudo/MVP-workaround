# MVP Architecture

## Objective

Build a working startup readiness funnel with:

- Free diagnostic
- Paid $9 roadmap
- Premium consult upsell

The MVP should validate whether founders are willing to exchange an email and pay for a personalized startup roadmap.

## System flow

1. User lands on website.
2. User starts free Tally assessment.
3. User completes Q1–Q15.
4. User enters email.
5. Submission is stored in Airtable.
6. Make.com computes or triggers scoring.
7. Free result is generated.
8. User sees score, top gaps, and upsell to paid roadmap.
9. If user pays, Stripe triggers Make.com.
10. Make.com retrieves assessment from Airtable.
11. Make.com sends Q1–Q20 data to OpenAI.
12. OpenAI returns structured roadmap.
13. Roadmap is saved to Airtable.
14. PDF is generated through Documint or PDF.co.
15. Email is sent through Resend or Brevo.
16. User receives roadmap and consult upsell.

## Airtable tables

### assessments

Stores free and paid answers, scores, generated roadmap, metadata, and delivery status.

### payments

Stores Stripe payment events and links each payment to an assessment.

### benchmarks_static

Stores simple benchmark references by stage, industry, region, and metric.

### consult_forms

Stores extended consult-intake answers.

### events

Stores funnel analytics events.

### prompt_versions

Stores active prompt versions for reproducibility.

## Key events

- assessment_started
- assessment_completed
- email_captured
- roadmap_purchased
- roadmap_generated
- pdf_generated
- pdf_downloaded
- consult_clicked
- consult_booked

## MVP constraints

Do not overbuild. The first working version should prioritize:

1. Reliable data capture
2. Clear scoring
3. Useful roadmap output
4. Paid conversion tracking
5. Manual override when automation fails
