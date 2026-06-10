# Make.com Scenarios v1

## Scenario 1: Free assessment submission

Trigger: Tally submission for Q1–Q15.

Steps:

1. Receive Tally webhook.
2. Create record in Airtable `assessments`.
3. Compute category scores and total score.
4. Determine top gaps.
5. Create `assessment_completed` event.
6. Send data to OpenAI using free assessment prompt.
7. Save summary, top gaps, and benchmark summary to Airtable.
8. Redirect or email user to free result page.

## Scenario 2: Paid roadmap purchase

Trigger: Stripe successful payment.

Steps:

1. Receive Stripe webhook.
2. Match payment to assessment using email or assessment_id.
3. Create record in `payments`.
4. Update `assessments.is_paid = true`.
5. Create `roadmap_purchased` event.
6. Send Q1–Q20 data to OpenAI using paid roadmap prompt.
7. Save generated roadmap to Airtable.
8. Generate PDF through Documint or PDF.co.
9. Save PDF link in Airtable.
10. Send delivery email through Resend or Brevo.
11. Create `pdf_generated` and `roadmap_delivered` events.

## Scenario 3: Consult booking

Trigger: Tally consult form or Calendly booking.

Steps:

1. Receive consult form submission.
2. Create record in `consult_forms`.
3. Create `consult_clicked` or `consult_booked` event.
4. Notify founder/operator by email.
5. Optional: send consult summary to OpenAI for internal prep notes.
