# React Report — Component Patterns & Chart Recipes

Use these patterns when generating a React artifact report. All imports use the libraries
available in Claude artifacts: `recharts`, `lucide-react`, and Tailwind utility classes.

---

## Layout Template

```jsx
import { useState } from "react";
import { BarChart, Bar, LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, ResponsiveContainer, Legend } from "recharts";

export default function Report() {
  const data = [ /* parsed from user's query results */ ];

  return (
    <div className="bg-gray-50 min-h-screen p-6 font-sans">
      {/* Header */}
      <div className="mb-6">
        <h1 className="text-2xl font-bold text-gray-900">Report Title</h1>
        <p className="text-gray-500 text-sm mt-1">Based on {data.length} rows · Generated June 2025</p>
      </div>

      {/* KPI Cards */}
      <div className="grid grid-cols-2 md:grid-cols-4 gap-4 mb-6">
        <KpiCard label="Total Revenue" value="$1.24M" delta="+12%" positive />
        {/* ... */}
      </div>

      {/* Charts */}
      <div className="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
        <ChartCard title="Revenue Over Time">
          {/* Recharts LineChart */}
        </ChartCard>
        <ChartCard title="Top Categories">
          {/* Recharts BarChart */}
        </ChartCard>
      </div>

      {/* Data Table */}
      <DataTable data={data} />
    </div>
  );
}
```

---

## KPI Card Component

```jsx
function KpiCard({ label, value, delta, positive }) {
  return (
    <div className="bg-white rounded-xl p-4 shadow-sm border border-gray-100">
      <p className="text-xs text-gray-500 uppercase tracking-wide">{label}</p>
      <p className="text-2xl font-bold text-gray-900 mt-1">{value}</p>
      {delta && (
        <p className={`text-xs mt-1 font-medium ${positive ? "text-green-600" : "text-red-500"}`}>
          {delta} vs prior period
        </p>
      )}
    </div>
  );
}
```

---

## Chart Card Wrapper

```jsx
function ChartCard({ title, children }) {
  return (
    <div className="bg-white rounded-xl p-4 shadow-sm border border-gray-100">
      <h2 className="text-sm font-semibold text-gray-700 mb-4">{title}</h2>
      <div className="h-56">
        <ResponsiveContainer width="100%" height="100%">
          {children}
        </ResponsiveContainer>
      </div>
    </div>
  );
}
```

---

## Chart Recipes

### Line Chart (trends over time)
```jsx
<LineChart data={data}>
  <CartesianGrid strokeDasharray="3 3" stroke="#f0f0f0" />
  <XAxis dataKey="date" tick={{ fontSize: 11 }} />
  <YAxis tick={{ fontSize: 11 }} />
  <Tooltip formatter={(v) => `$${v.toLocaleString()}`} />
  <Line type="monotone" dataKey="revenue" stroke="#6366f1" strokeWidth={2} dot={false} />
</LineChart>
```

### Bar Chart (category comparison)
```jsx
<BarChart data={topN} layout="vertical">
  <CartesianGrid strokeDasharray="3 3" stroke="#f0f0f0" horizontal={false} />
  <XAxis type="number" tick={{ fontSize: 11 }} />
  <YAxis type="category" dataKey="name" width={100} tick={{ fontSize: 11 }} />
  <Tooltip />
  <Bar dataKey="value" fill="#6366f1" radius={[0, 4, 4, 0]} />
</BarChart>
```

### Stacked Bar (composition over time)
```jsx
<BarChart data={data}>
  <CartesianGrid strokeDasharray="3 3" />
  <XAxis dataKey="month" />
  <YAxis />
  <Tooltip />
  <Legend />
  <Bar dataKey="categoryA" stackId="a" fill="#6366f1" />
  <Bar dataKey="categoryB" stackId="a" fill="#a5b4fc" />
  <Bar dataKey="categoryC" stackId="a" fill="#e0e7ff" />
</BarChart>
```

### Stat-only (no chart needed)
When you have a single aggregate and no time dimension, skip the chart and use a large KPI card instead.

---

## Data Table Component

```jsx
function DataTable({ data, limit = 20 }) {
  const [showAll, setShowAll] = useState(false);
  if (!data.length) return null;
  const cols = Object.keys(data[0]);
  const rows = showAll ? data : data.slice(0, limit);

  return (
    <div className="bg-white rounded-xl shadow-sm border border-gray-100 overflow-hidden">
      <div className="px-4 py-3 border-b border-gray-100 flex justify-between items-center">
        <h2 className="text-sm font-semibold text-gray-700">Full Data</h2>
        <span className="text-xs text-gray-400">{data.length} rows</span>
      </div>
      <div className="overflow-x-auto">
        <table className="w-full text-sm">
          <thead>
            <tr className="bg-gray-50">
              {cols.map(c => (
                <th key={c} className="text-left px-4 py-2 text-xs font-medium text-gray-500 uppercase tracking-wide">{c}</th>
              ))}
            </tr>
          </thead>
          <tbody>
            {rows.map((row, i) => (
              <tr key={i} className={i % 2 === 0 ? "bg-white" : "bg-gray-50"}>
                {cols.map(c => (
                  <td key={c} className="px-4 py-2 text-gray-700">{row[c] ?? "—"}</td>
                ))}
              </tr>
            ))}
          </tbody>
        </table>
      </div>
      {!showAll && data.length > limit && (
        <div className="px-4 py-3 border-t border-gray-100 text-center">
          <button onClick={() => setShowAll(true)} className="text-xs text-indigo-600 hover:underline">
            Show all {data.length} rows
          </button>
        </div>
      )}
    </div>
  );
}
```

---

## Number Formatting Helpers

```js
const fmt = {
  currency: (v) => `$${Number(v).toLocaleString("en-US", { minimumFractionDigits: 2, maximumFractionDigits: 2 })}`,
  integer: (v) => Number(v).toLocaleString("en-US"),
  percent: (v) => `${Number(v).toFixed(1)}%`,
  compact: (v) => {
    if (v >= 1_000_000) return `${(v / 1_000_000).toFixed(1)}M`;
    if (v >= 1_000) return `${(v / 1_000).toFixed(1)}K`;
    return String(v);
  }
};
```

---

## Color Palette

Use these Tailwind + hex values for chart colors (consistent across reports):

```
Primary:   #6366f1  (indigo-500)
Secondary: #10b981  (emerald-500)
Tertiary:  #f59e0b  (amber-500)
Danger:    #ef4444  (red-500)
Neutral:   #94a3b8  (slate-400)

Multi-series: ["#6366f1", "#10b981", "#f59e0b", "#3b82f6", "#ec4899"]
```

---

## Embedding Data

Hardcode parsed data directly in the component as a `const` array. Do not fetch from a URL.
Parse the user's CSV/JSON input and embed the cleaned array:

```jsx
const data = [
  { date: "2024-01", revenue: 42300, orders: 187 },
  { date: "2024-02", revenue: 51200, orders: 214 },
  // ...
];
```

If the dataset is large (> 200 rows), truncate to the top 200 for chart rendering but keep all rows for the table.
