# Monthly Ads Report Workflow

A monthly report should help a decision maker understand what happened, why it matters, and what to do next. It should not be a screenshot dump or a generic list of metrics.

Use this workflow when preparing a client, founder, or internal paid media report from Google Ads, Meta Ads, dashboard data, or CSV exports.

## Outcome

A useful monthly ads report answers five questions:

1. What changed this month?
2. Which campaigns, markets, services, or channels drove the change?
3. What is reliable signal versus noise?
4. What should we do next?
5. What needs tracking or data cleanup before the next report?

## Inputs

Collect these before writing analysis:

- Reporting period and comparison period.
- Spend, impressions, clicks, conversions, revenue or value where available.
- Campaign-level rows by platform.
- Service, market, or product grouping rules.
- Notes on launches, pauses, budget changes, tracking changes, and offer changes.
- Any known attribution gaps or conversion tracking issues.

## Step 1: Confirm The Reporting Scope

Before analysis, lock the basics:

- Current period: start and end date.
- Comparison period: previous month, previous 30 days, or same period last year.
- Platforms included: Google Ads, Meta Ads, or combined.
- Currency and timezone.
- Conversion definition.

If these are unclear, the rest of the report will look precise while being wrong.

## Step 2: Run Data QA

Do not write insights until the data passes basic checks.

Check:

- Spend totals match the source platforms or dashboard.
- Date ranges are correct and complete.
- No duplicated campaign rows inflate totals.
- Zero-spend and zero-conversion rows are handled intentionally.
- Google Ads micros are converted correctly.
- Meta and Google conversion definitions are not mixed without a note.
- Any tracking gap is documented before recommendations are written.

Use the [Marketing Data QA Checklist](marketing-data-qa-checklist.md) before moving on.

## Step 3: Build The Performance Snapshot

Create a simple top-level table:

| Metric | Current period | Comparison period | Change |
|---|---:|---:|---:|
| Spend |  |  |  |
| Impressions |  |  |  |
| Clicks |  |  |  |
| CTR |  |  |  |
| Conversions |  |  |  |
| CPL or CPA |  |  |  |
| Revenue or value |  |  |  |
| ROAS |  |  |  |

Use only metrics that are meaningful for the business. If revenue tracking is incomplete, do not make ROAS the main story.

## Step 4: Segment The Data

The useful story is usually hidden below the total.

Segment by:

- Platform: Google Ads, Meta Ads, other channels.
- Campaign or campaign type.
- Service, product, or offer.
- Market or geography.
- Funnel stage: awareness, traffic, lead generation, conversion.
- New versus existing campaigns when relevant.

Look for cases where the total looks stable but one segment is carrying or dragging performance.

## Step 5: Separate Signal From Noise

Not every percentage change deserves a recommendation.

Treat a change as stronger signal when it has:

- Meaningful spend.
- Enough clicks or conversions to support the conclusion.
- Consistent movement across multiple metrics.
- A clear operational cause, such as budget shift, new creative, landing page change, or tracking issue.

Treat a change cautiously when it has:

- Very low spend.
- Very few conversions.
- A short date range.
- Known tracking issues.
- A new campaign still in learning phase.

## Step 6: Write The Narrative

Use this structure:

```text
Overall summary:
What changed at the account level, in plain language.

Key drivers:
Which campaigns, services, markets, or channels explain the change.

Risks and gaps:
What may be distorted by tracking, small samples, or attribution limits.

Recommendations:
What to do next, ordered by impact.
```

Avoid vague statements such as "optimize performance" or "improve targeting." Make each recommendation specific enough that someone can act on it.

## Step 7: Make Recommendations Operational

Good recommendations include:

- The affected campaign, service, market, or platform.
- The metric that triggered the recommendation.
- The action to take.
- The expected reason this action helps.
- Any caution or dependency.

Example:

```text
Campaign A spent Rp 3,200,000 with a CPL 42% above account average and declining conversion volume. Reduce budget by 20% for the next seven days and shift the test budget into Campaign B, which has stronger lead efficiency at a similar spend level.
```

## Step 8: Close With Next Actions

End the report with a short action list:

| Priority | Action | Owner | Timing |
|---|---|---|---|
| High |  |  |  |
| Medium |  |  |  |
| Low |  |  |  |

If an action depends on better tracking, say that directly.

## Common Mistakes

Avoid:

- Writing the report before checking data quality.
- Explaining every chart instead of the few decisions that matter.
- Treating small sample changes as proven trends.
- Mixing platform conversions without noting definition differences.
- Making AI write recommendations from raw exports without signal filtering.
- Hiding tracking gaps because they make the story less clean.

## Final QA

Before sending:

- Month labels are correct.
- Totals match the source dashboard or export.
- Recommendations match the data shown.
- Tracking caveats are visible, not buried.
- No old client names, old dates, or template placeholders remain.
- The report makes clear what should happen next.

Final rule: the report is done when it helps someone make a better decision, not when every chart has a paragraph.
