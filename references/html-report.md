# HTML Report — Single-File Patterns with Chart.js

Use when the user wants a standalone `.html` file they can open in a browser or share via email.
Everything goes in one file — no build step, no dependencies to install.

---

## CDN Imports

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
```

---

## Full File Template

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Report Title</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif; background: #f8fafc; color: #1e293b; padding: 2rem; }
    h1 { font-size: 1.5rem; font-weight: 700; margin-bottom: .25rem; }
    .subtitle { color: #64748b; font-size: .875rem; margin-bottom: 2rem; }
    .kpi-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(160px, 1fr)); gap: 1rem; margin-bottom: 2rem; }
    .kpi-card { background: #fff; border-radius: 12px; padding: 1.25rem; box-shadow: 0 1px 3px rgba(0,0,0,.08); }
    .kpi-label { font-size: .7rem; text-transform: uppercase; letter-spacing: .06em; color: #94a3b8; }
    .kpi-value { font-size: 1.75rem; font-weight: 700; margin-top: .25rem; }
    .kpi-delta { font-size: .75rem; margin-top: .25rem; font-weight: 600; }
    .kpi-delta.pos { color: #10b981; } .kpi-delta.neg { color: #ef4444; }
    .chart-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(380px, 1fr)); gap: 1.5rem; margin-bottom: 2rem; }
    .chart-card { background: #fff; border-radius: 12px; padding: 1.25rem; box-shadow: 0 1px 3px rgba(0,0,0,.08); }
    .chart-card h2 { font-size: .875rem; font-weight: 600; color: #475569; margin-bottom: 1rem; }
    .chart-card canvas { max-height: 240px; }
    table { width: 100%; border-collapse: collapse; font-size: .875rem; }
    thead th { text-align: left; padding: .5rem 1rem; background: #f1f5f9; font-size: .7rem; text-transform: uppercase; letter-spacing: .05em; color: #64748b; }
    tbody tr:nth-child(even) { background: #f8fafc; }
    tbody td { padding: .5rem 1rem; border-bottom: 1px solid #e2e8f0; }
    .table-card { background: #fff; border-radius: 12px; box-shadow: 0 1px 3px rgba(0,0,0,.08); overflow: hidden; }
    .table-header { padding: .75rem 1rem; border-bottom: 1px solid #e2e8f0; display: flex; justify-content: space-between; align-items: center; }
    .table-header span { font-size: .875rem; font-weight: 600; color: #475569; }
    .table-header small { font-size: .75rem; color: #94a3b8; }
    .note { margin-top: 1.5rem; background: #fffbeb; border-left: 3px solid #f59e0b; padding: .75rem 1rem; border-radius: 0 8px 8px 0; font-size: .8rem; color: #92400e; }
  </style>
</head>
<body>
  <h1>Report Title</h1>
  <p class="subtitle">Data snapshot · June 2025 · N rows</p>

  <!-- KPIs -->
  <div class="kpi-grid">
    <div class="kpi-card">
      <div class="kpi-label">Total Revenue</div>
      <div class="kpi-value">$1.24M</div>
      <div class="kpi-delta pos">↑ 12% vs prior period</div>
    </div>
    <!-- repeat for each KPI -->
  </div>

  <!-- Charts -->
  <div class="chart-grid">
    <div class="chart-card">
      <h2>Revenue Over Time</h2>
      <canvas id="trendChart"></canvas>
    </div>
    <div class="chart-card">
      <h2>Top Categories</h2>
      <canvas id="barChart"></canvas>
    </div>
  </div>

  <!-- Table -->
  <div class="table-card">
    <div class="table-header">
      <span>Full Data</span>
      <small>20 rows shown</small>
    </div>
    <table id="dataTable"></table>
  </div>

  <script>
    // ---- DATA (hardcoded from user's query results) ----
    const trendData = {
      labels: ["Jan", "Feb", "Mar", "Apr"],
      values: [42300, 51200, 48900, 63100]
    };
    const barData = {
      labels: ["Product A", "Product B", "Product C"],
      values: [63100, 48900, 31200]
    };

    // ---- CHARTS ----
    const COLORS = { primary: "#6366f1", secondary: "#10b981", amber: "#f59e0b" };

    new Chart(document.getElementById("trendChart"), {
      type: "line",
      data: {
        labels: trendData.labels,
        datasets: [{ label: "Revenue", data: trendData.values, borderColor: COLORS.primary, backgroundColor: COLORS.primary + "20", tension: 0.3, pointRadius: 4 }]
      },
      options: { plugins: { legend: { display: false } }, scales: { y: { ticks: { callback: v => "$" + (v/1000).toFixed(0) + "K" } } }, responsive: true, maintainAspectRatio: true }
    });

    new Chart(document.getElementById("barChart"), {
      type: "bar",
      data: {
        labels: barData.labels,
        datasets: [{ data: barData.values, backgroundColor: COLORS.primary, borderRadius: 4 }]
      },
      options: { indexAxis: "y", plugins: { legend: { display: false } }, responsive: true, maintainAspectRatio: true }
    });
  </script>
</body>
</html>
```

---

## Chart.js Configuration Tips

- Always set `responsive: true` and `maintainAspectRatio: true` to avoid canvas overflow.
- Use `indexAxis: "y"` for horizontal bar charts (better for long category names).
- Format Y-axis ticks via `ticks.callback` — never display raw large numbers.
- Use `tension: 0.3` on line charts for smooth curves; `tension: 0` for exact values.
- For multiple series, use the color palette: `["#6366f1","#10b981","#f59e0b","#3b82f6","#ec4899"]`.
