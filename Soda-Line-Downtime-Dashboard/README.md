# Soda Bottling Line — Downtime & Efficiency Dashboard

An end-to-end Excel analytics project: raw CSV exports are cleaned, modeled, and turned into a KPI dashboard that explains where a bottling line is losing time and what to do about it.

**Pipeline:** Data Cleaning → KPIs → PivotTables → Charts → Dashboard → Business Insights

![Dashboard preview](readme_assets/dashboard_preview.png)

---

## 📌 Project Overview

A soda bottling line logs every batch it runs (product, operator, start/end time) and every minute of downtime against 12 possible causes (machine failure, batch change, inventory shortage, etc.). The raw exports are messy and split across five files with no relationship between them beyond a shared batch ID.

This project builds a single Excel workbook that:
- Cleans and reconciles the five source files into two tidy fact tables
- Calculates 38 KPIs covering production time, downtime, efficiency, and root causes
- Summarizes the data through six PivotTable-style breakdowns (by cause, operator, product, and day)
- Visualizes the findings on a one-page executive dashboard
- Translates the numbers into 11 prioritized, actionable business recommendations

Every figure in the workbook — KPIs, tables, charts, and even the insight sentences — is a **live formula**. Nothing is hard-coded, so the whole workbook recalculates if the source data changes.

## 🎯 Business Questions Answered

- How much production time is lost to downtime, and what would fixing it be worth?
- Which downtime causes matter most, and how many causes drive 80% of the loss?
- How much of the downtime is operator-controllable vs. equipment/materials?
- Do operators or products differ meaningfully in performance?
- What's the realistic upside of a targeted improvement effort?

## 🗂️ Dataset

Five CSV exports from the line's production and downtime logs:

| File | Contents |
|---|---|
| `line-productivity.csv` | One row per batch: date, product, operator, start/end time |
| `line-downtime.csv` | Downtime minutes per batch, one column per cause (wide format) |
| `products.csv` | Product reference table: flavor, size, standard (minimum) batch time |
| `downtime-factors.csv` | Downtime cause reference table, flagged operator error yes/no |
| `metadata.csv` | Field definitions for the source files |

**Known data issues, all handled in the workbook** (see the `Cleaning_Log` tab for full detail):
- Inconsistent delimiters across files (pipe vs. comma)
- A duplicate header row embedded inside the downtime data
- Wide-format downtime table that needed unpivoting into a tidy structure
- A midnight-crossing batch with a corrupted end-timestamp
- 7 batches present in the downtime log with no matching production record (excluded from KPIs, kept and flagged in the raw table)

## 🛠️ Built With

- **Microsoft Excel** — formulas (`INDEX/MATCH`, `SUMIFS`, `COUNTIFS`, `RANK`), native Tables, conditional formatting, combo charts
- **Python (pandas)** — used during development to independently verify every KPI and reconciliation check before finalizing the Excel formulas

## 📊 Workbook Structure

| Sheet | Purpose |
|---|---|
| `Dashboard` | One-page summary: 6 KPI cards, 4 charts, key-insights strip |
| `Insights` | 11 findings with recommended actions, priority ranking, and data caveats |
| `KPIs` | 38 KPIs with definitions, organized by theme, plus an editable what-if scenario |
| `Pivot_Tables` | 6 breakdowns: Pareto by cause, operator-error split, by operator, by product, daily trend, cause × operator heat-map |
| `Clean_Data` | Cleaned fact table — one row per batch (Excel Table) |
| `Downtime_Long` | Cleaned, unpivoted downtime records — one row per batch × cause (Excel Table) |
| `Lookups` | Product and downtime-cause reference tables |
| `Cleaning_Log` | Every data issue found, the fix applied, and 8 live validation checks |
| `Data_Dictionary` | Field-level documentation for every column in the model |

## 🔎 Key Insights

- Downtime consumes **35.5%** of production time (1,130 of 3,180 minutes across 31 batches), holding line efficiency to **64.5%**.
- Just **5 of 12** downtime causes account for **80%** of lost time — led by machine failure, inventory shortage, and machine adjustment.
- **51.6%** of downtime is operator-controllable, with a clear performance gap between the highest- and lowest-downtime operators.
- Downtime rates are consistent across products (32%–36%), pointing to line-wide process issues rather than a single problem product.
- A modeled improvement scenario (cutting operator-error downtime 50% and other downtime 25%) would lift efficiency from 64.5% to ~74.5% — roughly 6–7 extra standard batches of capacity.

*(Full detail, caveats, and recommended actions are in the `Insights` tab.)*

## 📁 Repository Contents

```
├── Soda_Line_Downtime_Dashboard.xlsx   # the full workbook
├── /data                               # raw source CSVs
└── /readme_assets                      # preview images used in this README
```

## 🚀 How to Use

1. Download `Soda_Line_Downtime_Dashboard.xlsx` and open it in Excel (Excel 2016+ recommended for full chart/formula support).
2. Start on the `Dashboard` tab for the executive summary.
3. Drill into `Pivot_Tables` and `KPIs` for supporting detail, and `Insights` for recommendations.
4. To try the what-if scenario, edit the two yellow input cells at the bottom of the `KPIs` tab — every dependent figure recalculates automatically.
5. To trace how any number was built, check `Cleaning_Log` (data fixes and validation) and `Data_Dictionary` (field definitions).

## ⚠️ Limitations

- Small sample: 31 batches across 5 production days and 4 operators — operator/product comparisons are directional, not statistically conclusive.
- Comparisons are not adjusted for product mix, shift timing, or equipment condition.
- The what-if scenario's improvement percentages are illustrative assumptions, not derived targets — replace them with agreed goals before using for planning.

---

*Built as part of a data analytics portfolio project — data cleaning, KPI design, and dashboarding in Excel.*
