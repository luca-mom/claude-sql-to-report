---
name: sql-to-report
description: >
  Transform SQL query results and schema context into polished, human-readable analysis reports
  with charts, narrative insights, and formatted tables. Use this skill whenever the user shares
  CSV data, query results, a database schema, or asks to "analyze this data", "make a report from
  this query", "visualize my SQL output", or "turn this into a dashboard". Trigger even when SQL
  isn't explicitly mentioned — if the user pastes tabular data and wants insight or a report,
  this skill applies. Always use for: data summaries, KPI reports, trend analysis, cohort breakdowns,
  funnel analysis, and any task that turns raw query output into business-readable output.
---

# SQL-to-Report Skill

You receive raw SQL query results (CSV, JSON, or pasted table), optionally with schema context or
business descriptions. Your job: produce a polished analysis report with narrative, charts, and
formatted tables that a non-technical stakeholder can understand.

## Input Types

The user may provide any combination of:
- **Raw query output** — CSV rows, JSON arrays, pasted table text
- **Schema context** — table definitions, column descriptions, relationships
- **Query itself** — the SQL that produced the results
- **Business context** — what the data is about, who the audience is
- **Report format preference** — React artifact, HTML file, Markdown, or .docx

If the user provides data but no format preference, default to a **React artifact** (rich, interactive).

---

## Step 1 — Understand the Data

Before writing any report:

1. **Infer the domain** — Is this sales, finance, product usage, operations, marketing? Name it.
2. **Identify the grain** — What does one row represent? (one user, one transaction, one day?)
3. **Spot key metrics** — Which columns are measures (numbers to aggregate) vs. dimensions (things to group by)?
4. **Detect time columns** — Are there dates/timestamps? If so, trend analysis is possible.
5. **Check for anomalies** — Nulls, outliers, suspiciously round numbers, or data quality issues worth flagging.

State your interpretation briefly before proceeding — e.g., *"This looks like daily revenue by product category over Q1 2024."* Ask for clarification only if the grain or purpose is genuinely ambiguous.

---

## Step 2 — Plan the Report Structure

A good report has 3–5 sections. Choose from:

| Section | When to include |
|---|---|
| **Executive Summary** | Always. 3–5 bullet KPIs at the top. |
| **Trend Over Time** | When a date/timestamp column exists |
| **Top N / Rankings** | When there's a categorical dimension + a numeric measure |
| **Distribution / Breakdown** | When you want to show composition (pie, stacked bar) |
| **Cohort / Segment Analysis** | When user/group IDs allow segmentation |
| **Anomalies & Callouts** | When outliers or data quality issues are notable |
| **Appendix: Full Table** | Always for < 500 rows; optional for larger datasets |

Do NOT include sections that the data can't support. A 3-section tight report beats a 6-section padded one.

---

## Step 3 — Choose the Output Format

### React Artifact (default — richest output)
Use when: the user wants interactive charts, filters, or a dashboard feel.
→ See `references/react-report.md` for component patterns and chart recipes.

### HTML File
Use when: the user wants a standalone file to share or open in a browser.
→ Use Chart.js via CDN. Single file, no build step. See `references/html-report.md`.

### Markdown
Use when: the user wants something to paste into Notion, GitHub, or a doc.
→ Use tables and code blocks. No charts (use ASCII sparklines for trends if helpful).

### .docx
Use when: the user explicitly asks for Word, or signals a formal deliverable ("send to client", "board report").
→ Read the `docx` skill first (`/mnt/skills/public/docx/SKILL.md`), then generate.

---

## Step 4 — Write the Report

### Narrative Voice
- Write like a data analyst presenting to a business audience — confident, specific, no jargon.
- Lead with the "so what", not the "what". Bad: *"Revenue was $1.2M in March."* Good: *"March was the strongest month of Q1, driven by a 34% spike in Enterprise plan upgrades."*
- Call out the most surprising or actionable finding in the first 2 sentences.
- Use **bold** for numbers that matter.

### Chart Selection Guide
| Goal | Chart type |
|---|---|
| Trend over time | Line chart |
| Compare categories | Bar chart (horizontal if > 6 categories) |
| Show composition | Stacked bar or pie (pie only if ≤ 5 slices) |
| Correlation | Scatter plot |
| Distribution | Histogram or box plot |
| Single KPI | Stat card with delta |

### KPI Cards (Executive Summary)
Always include 3–5 KPI stat cards at the top:
- Total / sum of the primary metric
- Period-over-period change (if time data exists)
- Top performer (top category, top user, etc.)
- One "health" metric (conversion rate, churn %, error rate, etc.) if inferable

### Table Formatting
- Sort by the most important metric descending by default
- Limit to top 20 rows in the visible table; offer "show all" toggle in React
- Format numbers: use commas for thousands, 2 decimal places for currency, 1 for percentages
- Highlight the top row or max value in each column

---

## Step 5 — Data Quality Notes

If you notice any of the following, add a short **⚠️ Data Notes** section:
- Nulls in key columns (flag count and %)
- Date gaps (missing days/months in a time series)
- Duplicate rows
- Values that look like test data ("test@test.com", id=0, etc.)
- Columns that appear miscategorized

Do NOT refuse to generate the report because of data quality issues — note them and proceed.

---

## Quality Checklist

Before finalizing:
- [ ] Every chart has a title and labeled axes
- [ ] Numbers are formatted (commas, %, $ as appropriate)
- [ ] The narrative says *why* something is interesting, not just *what* it is
- [ ] KPI cards are at the top
- [ ] No section is included that the data doesn't support
- [ ] Anomalies or nulls are noted if present
- [ ] The report could be understood by someone who never saw the SQL

---

## Reference Files

- `references/react-report.md` — React component patterns, Recharts recipes, layout templates
- `references/html-report.md` — Single-file HTML patterns with Chart.js
- `references/narrative-phrases.md` — Pre-written analyst phrases for common findings

Read the relevant reference file for your chosen output format before writing code.
