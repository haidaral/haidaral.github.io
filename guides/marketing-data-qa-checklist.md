# Marketing Data QA Checklist

Marketing reports fail quietly when the data is wrong. This checklist catches the most common issues before numbers become insights, recommendations, or automated alerts.

Use it before monthly reports, dashboard releases, AI insight generation, and scheduled bot messages.

## Quick Pass

Run this quick pass before doing deeper analysis:

- The reporting date range is correct.
- The comparison period is correct.
- Spend totals match the source platform or expected dashboard total.
- Currency is consistent.
- Timezone is known.
- Conversions mean the same thing across views, or differences are documented.
- No private credentials, account IDs, or raw client exports are being shared publicly.

If any item fails, pause analysis and fix the data first.

## 1. Date Range Checks

Confirm:

- Start and end dates match the report title.
- The comparison period is the intended one.
- Platform exports use the same timezone.
- Partial current-day data is not mixed with complete previous-day data.
- Weekly and monthly views include the same boundaries.

Common issue: a dashboard uses local timezone while a platform export uses account timezone, causing a one-day mismatch.

## 2. Spend And Currency Checks

Confirm:

- Google Ads micros are converted to standard currency units.
- Meta Ads spend is already in the expected currency.
- Combined platform totals do not mix currencies without conversion.
- Rounded display totals still reconcile with source totals.
- Refunds, credits, or adjustments are understood when source totals differ.

Sanity check:

```text
If spend is 1,000,000 times too high or too low, check micros conversion first.
```

## 3. Duplicate Row Checks

Look for duplicate rows caused by joins, repeated exports, or grouping mistakes.

Check duplicates by:

- Campaign ID plus date.
- Campaign name plus date if ID is unavailable.
- Platform plus campaign plus date.
- Ad set or ad group plus date when using lower-grain data.

Warning signs:

- Spend doubles after adding campaign labels.
- Conversions increase after joining taxonomy data.
- Row count jumps unexpectedly after merging two tables.

## 4. Metric Formula Checks

Use consistent formulas:

```text
CTR = clicks / impressions
CPL or CPA = spend / conversions
ROAS = revenue / spend
CPM = spend / impressions * 1000
Conversion rate = conversions / clicks
```

Handle zero denominators intentionally. Do not let `inf`, `NaN`, or blank values silently appear in client-facing tables.

## 5. Conversion Definition Checks

Ask:

- What counts as a conversion?
- Is it a lead, purchase, form submit, booked call, or platform-optimized event?
- Are Google Ads and Meta Ads counting the same business action?
- Are view-through conversions included?
- Did the conversion setup change during the period?

If definitions differ, avoid pretending the combined conversion total is a clean business truth.

## 6. Tracking Gap Checks

Flag possible tracking issues when:

- Clicks are stable but conversions suddenly drop to zero.
- One platform reports conversions while analytics does not.
- A known form, CRM, or landing page change happened during the period.
- Conversion volume shifts sharply without a matching spend, traffic, or creative change.
- A campaign with historically stable performance suddenly reports no leads.

Tracking caveat template:

```text
Conversion volume should be interpreted cautiously this period because tracking changes or reporting gaps may be affecting the platform-reported totals.
```

## 7. Outlier Checks

Investigate outliers before writing insights.

Look for:

- Extremely high CTR with very low conversions.
- Very low CPC that produces poor lead quality.
- High spend with zero conversions.
- A single campaign driving most spend or conversions.
- A campaign with performance that is too good to be true.

Outliers are not automatically bad. They are a prompt to verify.

## 8. Segmentation Checks

When segmenting by service, market, product, or funnel stage, confirm:

- Every campaign maps to one expected category or an explicit `Unknown` bucket.
- Unknown rows are reviewed before final reporting.
- Market codes and service codes come from a consistent taxonomy.
- Combined totals still match after segmentation.

A category split is useful only if the mapping is trustworthy.

## 9. AI Readiness Checks

Before sending data to an AI model, confirm:

- Metrics are already calculated by code.
- Rows are filtered to meaningful spend or signal thresholds.
- Campaigns include labels such as platform, market, service, and detected signal.
- Known tracking caveats are included explicitly.
- The model is asked for structured output, not vague prose.
- Private data and secrets are excluded.

AI should explain and prioritize. It should not calculate core metrics from raw exports.

## 10. Report Readiness Checks

Before publishing or sending a report:

- The executive summary matches the tables.
- Recommendations point to real numbers in the report.
- Small-sample caveats are visible.
- No old month labels remain.
- No placeholder text remains.
- No private notes or internal-only comments are visible.
- The next actions are specific enough to execute.

## Common Failure Patterns

| Symptom | Likely cause |
|---|---|
| Spend is wildly too high | Google micros not converted |
| Totals doubled after a merge | Duplicate join keys |
| ROAS looks perfect but revenue is blank | Missing revenue field handled as zero or stale value |
| AI recommendations feel generic | Data was not signal-tagged before prompting |
| One market looks broken | Taxonomy or campaign naming mismatch |
| Report says "conversion drop" but clicks also dropped | Traffic volume changed, not necessarily conversion efficiency |

## Final Rule

Do not explain bad data beautifully. Fix it, caveat it, or leave it out.
