# Startup Readiness MVP

A lightweight MVP for assessing startup readiness, identifying execution gaps, and selling a paid roadmap.

## Funnel

1. Free assessment
   - 15-question Tally form
   - Email capture before results
   - Score out of 100
   - Top 2–3 gaps
   - Basic benchmark snapshot
   - Upsell to $9 personalized roadmap

2. Paid roadmap
   - Same 15-question base assessment
   - 5 extra paid questions
   - AI-generated roadmap
   - Downloadable PDF/doc
   - Delivery by email

3. Premium consult
   - Extended 25–30 question pre-call form
   - Human review
   - Strategy, execution, fundraising, hiring, or growth support

## Stack

- Landing page: Framer or Carrd
- Forms: Tally
- Database: Airtable
- Automation: Make.com
- Payments: Stripe
- AI generation: OpenAI
- PDF generation: Documint or PDF.co
- Email: Resend or Brevo
- Analytics: PostHog or Airtable events table
- Scheduling: Calendly

## Airtable tables

- assessments
- payments
- benchmarks_static
- consult_forms
- events
- prompt_versions

## Versioning

- scoring_model_version: v1
- prompt_version: v1
- roadmap_version: v1

## Product boundary

This is not investment advice, fundraising advice, or a startup valuation. It is a structured readiness diagnostic and execution-planning tool.
