# Narrative Phrases — Analyst Language for Common Findings

Use these as templates. Replace **[bold placeholders]** with actual values.

---

## Executive Summary Openers

- "**[Metric]** reached **[value]** in **[period]**, the **[highest/lowest]** point in the dataset."
- "The data tells a story of **[growth/decline/volatility]**: **[metric]** shifted **[direction]** across **[period]**, driven primarily by **[top factor]**."
- "At a glance: strong performance in **[area]**, offset by weakness in **[area]**."

---

## Trend Language

**Positive trend:**
- "**[Metric]** climbed steadily from **[start]** to **[end]**, a **[X]%** increase over **[period]**."
- "Month-over-month growth averaged **[X]%**, with the sharpest acceleration in **[period]**."

**Negative trend:**
- "**[Metric]** declined **[X]%** from **[start]** to **[end]** — a trend worth monitoring."
- "After peaking at **[value]** in **[period]**, **[metric]** fell back to **[value]**."

**Flat / stable:**
- "**[Metric]** remained largely stable across the period, fluctuating within a **[X]–[Y]** band."

**Volatile:**
- "**[Metric]** showed high variability, swinging between **[min]** and **[max]** — suggesting sensitivity to **[possible cause]**."

---

## Rankings / Top-N Language

- "**[Top item]** led the field with **[value]**, accounting for **[X]%** of the total."
- "The top 3 categories — **[A]**, **[B]**, and **[C]** — together represent **[X]%** of **[metric]**."
- "**[Bottom item]** lagged significantly, contributing just **[X]%** despite being the **[N]th** largest segment."
- "There's a pronounced concentration effect: the top **[N]** items capture **[X]%** of **[metric]**, while the remaining **[M]** share the rest."

---

## Comparison / Period-over-Period

- "Compared to the prior period, **[metric]** improved by **[X]%** (**[old value]** → **[new value]**)."
- "**[Segment A]** outperformed **[Segment B]** by a factor of **[X]**, suggesting **[possible insight]**."
- "The gap between **[best]** and **[worst]** performers widened in **[period]**, reaching **[X]** — the largest spread in the dataset."

---

## Data Quality / Caveats

- "**⚠️ Note:** **[X]** rows (**[Y]%**) have null values in **[column]** and were excluded from **[calculation]**."
- "**⚠️ Note:** The data contains a gap between **[date A]** and **[date B]**; the trend line may be misleading across that window."
- "**⚠️ Note:** **[Column]** contains what appear to be test records (values like **[example]**). These have been excluded."

---

## Closing / Recommendations Section (optional)

Use only when the data clearly supports an actionable takeaway:

- "Given the concentration in **[top segment]**, there may be an opportunity to diversify by investing in **[underperforming segment]**."
- "The drop-off in **[metric]** after **[date]** warrants further investigation — consider cross-referencing with **[related data source]**."
- "The data does not yet support a clear directional conclusion; a longer time window would strengthen the analysis."
