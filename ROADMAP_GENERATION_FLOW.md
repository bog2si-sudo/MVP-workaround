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
2. Retrieve active prompt from `prompt_versions`.
3. Retrieve benchmark row from `benchmarks_static`.
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
