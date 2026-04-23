# AI Insight Engine For Ads

An AI insight engine should not be a prompt wrapped around a CSV export. It should be a small decision system: clean data in, deterministic signals, structured AI output, and human-usable recommendations.

Use this guide when building AI-assisted analysis for Google Ads, Meta Ads, dashboards, monthly reports, or reporting bots.

## Core Rule

Code calculates and detects. AI explains and prioritizes.

Do not ask a model to calculate ROAS, CPL, CTR, trend deltas, or spend thresholds from raw exports. Calculate those with code first, then send the model a compact payload that already contains the important signals.

## Recommended Flow

```text
Raw platform data
-> normalize fields and currency
-> calculate metrics
-> detect signals
-> filter noisy rows
-> rank campaign priority
-> send compact JSON to AI
-> validate strict JSON response
-> format for report, dashboard, or bot
```

## Step 1: Normalize Data

Every row should use consistent names and units before analysis.

Recommended fields:

```json
{
  "campaign_name": "Example Campaign",
  "platform": "google",
  "market": "ID",
  "service": "Lead Gen",
  "spend": 1250000,
  "impressions": 45000,
  "clicks": 1200,
  "conversions": 24,
  "revenue": 4200000,
  "date_range": {
    "start": "2026-03-01",
    "end": "2026-03-31"
  }
}
```

Keep raw platform-specific fields out of the AI payload unless they are needed for a specific reason.

## Step 2: Calculate Metrics In Code

Use deterministic formulas:

```python
def safe_divide(numerator: float, denominator: float) -> float:
    return numerator / denominator if denominator else 0.0

row["ctr"] = safe_divide(row["clicks"], row["impressions"])
row["cpl"] = safe_divide(row["spend"], row["conversions"])
row["roas"] = safe_divide(row["revenue"], row["spend"])
```

Handle missing revenue carefully. If revenue tracking is incomplete, do not make ROAS the main signal.

## Step 3: Detect Signals

Signals are labels that explain why a campaign matters.

Example signal rules:

```python
THRESHOLDS = {
    "min_spend": 100_000,
    "high_spend": 1_000_000,
    "low_roas": 1.5,
    "strong_roas": 3.0,
    "high_cpl": 500_000,
    "low_ctr": 0.01,
}

def detect_signals(row: dict) -> list[str]:
    signals = []

    if row["spend"] < THRESHOLDS["min_spend"]:
        return signals

    if row["roas"] < THRESHOLDS["low_roas"]:
        signals.append("LOW_ROAS")

    if row["cpl"] > THRESHOLDS["high_cpl"]:
        signals.append("HIGH_CPL")

    if row["ctr"] < THRESHOLDS["low_ctr"]:
        signals.append("LOW_CTR")

    if row["spend"] > THRESHOLDS["high_spend"] and row["roas"] < THRESHOLDS["low_roas"]:
        signals.append("HIGH_SPEND_LOW_RETURN")

    if row["spend"] < THRESHOLDS["high_spend"] and row["roas"] > THRESHOLDS["strong_roas"]:
        signals.append("LOW_SPEND_HIGH_ROAS")

    return signals
```

Thresholds should be tuned to the account, market, offer, and currency. The exact numbers above are starting points, not universal truth.

## Step 4: Filter Noise

Do not send every campaign to the model.

Filter out:

- Very low spend rows.
- Rows with no useful signal.
- Campaigns with incomplete mandatory fields.
- Duplicate rows.
- Raw rows that have not passed data QA.

Most prompts work better with the top 5 to 10 meaningful campaign rows than with a full account export.

## Step 5: Build The AI Payload

Send compact, signal-tagged objects:

```json
[
  {
    "campaign_name": "Example Campaign",
    "platform": "google",
    "market": "ID",
    "service": "Lead Gen",
    "spend": 1250000,
    "roas": 1.2,
    "ctr_pct": 0.8,
    "cpl": 625000,
    "conversions": 2,
    "signals": ["LOW_ROAS", "HIGH_CPL", "HIGH_SPEND_LOW_RETURN"],
    "expected_priority": "high",
    "tracking_caveat": null
  }
]
```

If tracking is incomplete, include a caveat explicitly. Do not rely on the model to infer it.

## Step 6: Prompt For Strict JSON

The system prompt should be short and strict:

```text
You are a performance marketing analyst. You receive campaign rows that were already normalized, calculated, and signal-tagged by code.

Rules:
- Use only the provided data.
- Reference real numbers in every issue.
- Recommend one concrete action per insight.
- Mention tracking caveats when present.
- Prioritize high spend and weak return.
- Output valid JSON only.
```

Require a schema:

```json
[
  {
    "campaign": "string",
    "platform": "google | meta | mixed",
    "issue": "metric-backed problem or opportunity",
    "impact": "business consequence",
    "recommendation": "one concrete next action",
    "priority": "high | medium | low",
    "confidence": "high | medium | low"
  }
]
```

Validate the response before using it in a dashboard, report, or bot.

## Step 7: Format For The Destination

Use different formatters for different outputs:

- Dashboard: concise cards with priority, metric, and action.
- Monthly report: polished narrative with caveats and business context.
- Telegram or Slack: short alerts with the action near the top.
- Internal QA: include raw signals and confidence.

The model should return structured insight data. The application should own final formatting.

## Stable-State Response

If no strong signals exist, return a handled stable-state message:

```json
[
  {
    "campaign": "all",
    "platform": "mixed",
    "issue": "No significant performance issue detected in the current period.",
    "impact": "Campaigns are operating within normal thresholds.",
    "recommendation": "Maintain current strategy and continue monitoring tracking quality.",
    "priority": "low",
    "confidence": "medium"
  }
]
```

This is better than forcing the AI to invent problems.

## Common Mistakes

Avoid:

- Sending raw CSV exports directly to the model.
- Asking the model to calculate core metrics.
- Producing free-form prose when the next system expects JSON.
- Generating insights for tiny spend rows.
- Treating tracking gaps as proven campaign failures.
- Expanding the prompt when the real issue is weak signal detection.
- Sending private client data, tokens, or account IDs to external systems unnecessarily.

## Minimum Build Checklist

A useful first version needs only:

- A normalized campaign dataframe.
- A metrics helper.
- A signal detection helper.
- A payload builder.
- One strict prompt.
- JSON validation.
- One formatter for the first destination.

Do not start with a large agent architecture. Start with a clear pipeline that produces useful recommendations.

## Final Rule

If the AI sounds generic, improve the evidence you send it. Better signals beat longer prompts.
