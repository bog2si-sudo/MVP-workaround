# Repository Files 2 to 10

This file contains files 2 to 10 for the Startup Readiness MVP repository.

To split later, create each filename/path shown below and paste its corresponding content.

---

# FILE 2

Path:

docs/mvp_architecture.md

Content:

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

---

# FILE 3

Path:

scoring/scoring_model_v1.md

Content:

# Scoring Model v1

Total score: 100 points.

## Categories

- Team: 20 points
- Market: 25 points
- Traction: 30 points
- Product & Economics: 25 points

## Team

### Q1. Built something before?

- No, first time: 0
- Yes, but not in this domain: 4
- Yes, in this domain: 7

### Q2. Co-founder/core team member with relevant domain experience?

- No: 0
- Partially: 4
- Yes, strong experience: 7

### Q3. Team execution ability next 6 months?

- Low: 0
- Medium: 3
- High: 6

Team maximum: 20.

## Market

### Q4. Market size?

- Local / very small: 2
- National / medium: 5
- Global / large: 8

### Q5. Problem urgency/pain?

- Not really, nice-to-have: 0
- Somewhat painful: 6
- Very painful, they already pay for solutions: 10

### Q6. Customer definition clarity?

- Not clear yet: 0
- Somewhat clear: 4
- Very clear, with examples: 7

Market maximum: 25.

## Traction

### Q7. Paying customers?

- No: 0
- Yes, 1–5 customers: 7
- Yes, 6+ customers: 10

### Q8. Monthly revenue?

- $0: 0
- <$1,000: 4
- $1,000–$5,000: 8
- $5,000–$20,000: 12
- >$20,000: 15

### Q9. Revenue repeatable?

- No, mostly one-off: 0
- Somewhat repeatable: 3
- Yes, clearly repeatable: 5

Traction maximum: 30.

## Product & Economics

### Q10. Differentiation vs existing solutions?

- Not differentiated: 0
- Slightly better: 5
- Clearly better: 9

### Q11. Know unit economics?

- No idea: 0
- Rough estimates: 5
- We measure them regularly: 8

### Q12. Distribution strength?

- Weak / unclear: 0
- Medium: 4
- Strong: 8

Product & Economics maximum: 25.

## Gap logic

A category is a major gap if it scores below 50% of its possible points.

Priority order:

1. Traction
2. Market
3. Product & Economics
4. Team

If two categories are tied, choose the one with greater near-term execution impact.

## Score bands

- 0–30: Fragile / idea-stage risk
- 31–55: Early potential, major validation gaps
- 56–75: Promising but execution-sensitive
- 76–90: Strong readiness
- 91–100: High readiness, focus on scale discipline

---

# FILE 4

Path:

prompts/free_assessment_prompt_v1.md

Content:

# Free Assessment Prompt v1

You are a startup readiness analyst.

Your task is to produce a concise free diagnostic based on a founder's assessment answers.

Do not give investment advice. Do not estimate company valuation. Do not make exaggerated claims.

## Input

You will receive:

- Stage
- Industry
- Region
- Q1–Q15 answers
- Category scores
- Total score
- Top gaps
- Benchmark snapshot, if available

## Output format

Return valid JSON only:

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

## Style rules

- Clear
- Direct
- Non-technical
- No hype
- No flattery
- Useful for a busy founder

## Diagnostic logic

Explain the score in practical terms.

Focus on:

- What is weak
- What is blocking progress
- What should be fixed first

The upsell copy should invite the user to buy the $9 roadmap for a deeper 30/60/90-day plan.

---

# FILE 5

Path:

prompts/paid_roadmap_prompt_v1.md

Content:

# Paid Roadmap Prompt v1

You are a startup readiness analyst creating a practical execution roadmap.

The user paid for a personalized roadmap. The output must be useful enough to justify the purchase.

Do not provide investment advice, legal advice, tax advice, or a valuation.

## Input

You will receive:

- Stage
- Industry
- Region
- Q1–Q20 answers
- Category scores
- Total score
- Top gaps
- Benchmark snapshot, if available

## Output format

Return valid JSON only:

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

## Rules

- Be specific.
- Do not invent metrics not present in the input.
- If data is missing, say what must be measured next.
- Prioritize action over theory.
- Use plain language.
- Avoid generic startup clichés.

---

# FILE 6

Path:

tally/tally_questions_v1.md

Content:

# Tally Questions v1

## Free assessment

### Q1. Built something before?

- No, first time
- Yes, but not in this domain
- Yes, in this domain

### Q2. Co-founder/core team member with relevant domain experience?

- No
- Partially
- Yes, strong experience

### Q3. Team execution ability next 6 months?

- Low
- Medium
- High

### Q4. Market size?

- Local / very small
- National / medium
- Global / large

### Q5. Problem urgency/pain?

- Not really, nice-to-have
- Somewhat painful
- Very painful, they already pay for solutions

### Q6. Customer definition clarity?

- Not clear yet
- Somewhat clear
- Very clear, with examples

### Q7. Paying customers?

- No
- Yes, 1–5 customers
- Yes, 6+ customers

### Q8. Monthly revenue?

- $0
- <$1,000
- $1,000–$5,000
- $5,000–$20,000
- >$20,000

### Q9. Revenue repeatable?

- No, mostly one-off
- Somewhat repeatable
- Yes, clearly repeatable

### Q10. Differentiation vs existing solutions?

- Not differentiated
- Slightly better
- Clearly better

### Q11. Know unit economics?

- No idea
- Rough estimates
- We measure them regularly

### Q12. Distribution strength?

- Weak / unclear
- Medium
- Strong

### Q13. Current stage?

- Pre-seed
- Seed
- Early growth
- Later growth

### Q14. Industry?

- SaaS
- E-commerce
- Marketplace
- Consumer app
- Services
- Other

### Q15. Region?

- EU
- US
- Other

## Paid add-on

### Q16. Biggest bottleneck?

- Team
- Product
- Traction
- Distribution
- Capital

### Q17. 6-month goal?

- Get first paying customers
- Scale to $10k/month
- Scale to $50k+/month
- Raise funding
- Other

### Q18. Weekly growth time?

- <5 hours
- 5–10 hours
- 10–20 hours
- 20+ hours

### Q19. Growth/marketing budget?

- $0
- <$500/month
- $500–$2,000/month
- >$2,000/month

### Q20. Investor/advisor access?

- No
- Some
- Yes, meaningful access

## Consult pre-call

Q21. Describe primary customer segments.

Q22. What have you tried to get customers? What worked and what failed?

Q23. Biggest failure or mistake so far, and what did you learn?

Q24. Are you currently raising funds? Target and timeline?

Q25. Top 3 competitors and your edge.

Q26. Current team structure.

Q27. Hiring plan for the next 6 months.

Q28. Funnel metrics, if known.

Q29. Top 3 risks or constraints.

Q30. What help do you want most: strategy, execution, fundraising, hiring, growth, or something else?

---

# FILE 7

Path:

make/make_scenarios_v1.md

Content:

# Make.com Scenarios v1

## Scenario 1: Free assessment submission

Trigger: Tally submission for Q1–Q15.

Steps:

1. Receive Tally webhook.
2. Create record in Airtable assessments.
3. Compute category scores and total score.
4. Determine top gaps.
5. Create assessment_completed event.
6. Send data to OpenAI using free assessment prompt.
7. Save summary, top gaps, and benchmark summary to Airtable.
8. Redirect or email user to free result page.

## Scenario 2: Paid roadmap purchase

Trigger: Stripe successful payment.

Steps:

1. Receive Stripe webhook.
2. Match payment to assessment using email or assessment_id.
3. Create record in payments.
4. Update assessments.is_paid = true.
5. Create roadmap_purchased event.
6. Send Q1–Q20 data to OpenAI using paid roadmap prompt.
7. Save generated roadmap to Airtable.
8. Generate PDF through Documint or PDF.co.
9. Save PDF link in Airtable.
10. Send delivery email through Resend or Brevo.
11. Create pdf_generated and roadmap_delivered events.

## Scenario 3: Consult booking

Trigger: Tally consult form or Calendly booking.

Steps:

1. Receive consult form submission.
2. Create record in consult_forms.
3. Create consult_clicked or consult_booked event.
4. Notify founder/operator by email.
5. Optional: send consult summary to OpenAI for internal prep notes.

---

# FILE 8

Path:

emails/roadmap_delivery_email.md

Content:

# Roadmap Delivery Email

Subject: Your Startup Readiness Roadmap is ready

Hi {{first_name_or_email}},

Your Startup Readiness Roadmap is ready.

Download it here:

{{pdf_link}}

Your current readiness score:

{{score_total}} / 100

Main gaps:

1. {{top_gap_1}}
2. {{top_gap_2}}
3. {{top_gap_3}}

The roadmap includes:

- Score and gap analysis
- Top 3 execution priorities
- 30/60/90-day plan
- Key risks
- Experiments to run next
- One-page founder memo

If you want a deeper human review, you can book a consult here:

{{consult_link}}

Best,
Startup Readiness

---

# FILE 9

Path:

pdf/roadmap_template_v1.md

Content:

# Startup Readiness Roadmap

## 1. Score & Gap Summary

Total score: {{score_total}} / 100

Readiness label: {{readiness_label}}

{{executive_summary}}

## 2. Category Analysis

### Team

{{team_analysis}}

### Market

{{market_analysis}}

### Traction

{{traction_analysis}}

### Product & Economics

{{product_economics_analysis}}

## 3. Top 3 Priorities

### Priority 1

{{priority_1}}

### Priority 2

{{priority_2}}

### Priority 3

{{priority_3}}

## 4. 30-Day Plan

{{thirty_day_plan}}

## 5. 60-Day Plan

{{sixty_day_plan}}

## 6. 90-Day Plan

{{ninety_day_plan}}

## 7. Risks

### Risk 1

{{risk_1}}

### Risk 2

{{risk_2}}

### Risk 3

{{risk_3}}

## 8. Experiments

### Experiment 1

{{experiment_1}}

### Experiment 2

{{experiment_2}}

### Experiment 3

{{experiment_3}}

## 9. Benchmark Snapshot

{{benchmark_summary}}

Benchmark confidence: {{benchmark_confidence}}

## 10. One-Page Founder Memo

{{one_page_memo}}

## Disclaimer

This roadmap is a structured diagnostic and execution-planning document. It is not investment, legal, tax, or fundraising advice.

---

# FILE 10

Path:

docs/roadmap_generation_flow.md

Content:

# Roadmap Generation Flow

## Purpose

Generate a useful paid roadmap from founder assessment data.

## Required inputs

- email
- q1 through q20
- stage
- industry
- region
- score_total
- score_team
- score_market
- score_traction
- score_product_econ
- top_gap_1
- top_gap_2
- top_gap_3
- benchmark_summary
- benchmark_confidence

## Generation steps

1. Retrieve assessment from Airtable.
2. Retrieve active prompt from prompt_versions.
3. Retrieve benchmark row from benchmarks_static.
4. Build structured OpenAI input.
5. Call paid roadmap prompt.
6. Parse JSON response.
7. Save generated fields back into Airtable.
8. Generate PDF using template.
9. Store PDF URL in Airtable.
10. Email user the roadmap.

## Validation rules

Before sending the roadmap:

- Confirm score_total exists.
- Confirm at least one top gap exists.
- Confirm roadmap JSON is valid.
- Confirm PDF link exists.
- Confirm payment status is successful.

## Manual fallback

If automation fails:

1. Copy the assessment answers from Airtable.
2. Paste them into the paid roadmap prompt manually.
3. Generate the roadmap.
4. Export as PDF.
5. Upload PDF manually.
6. Paste PDF link into Airtable.
7. Send delivery email manually.
