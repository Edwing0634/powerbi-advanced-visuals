# Installation Guide — Power BI Advanced Visuals

## Prerequisites

- Power BI Desktop (any version from 2023 onwards)
- **HTML Content** visual installed from AppSource (by Nova Silva)
- Internet access from Power BI Service (for CDN-loaded libraries)

## Step 1 — Install HTML Content Visual

1. In Power BI Desktop, click **...** in the Visualizations pane → **Get more visuals**
2. Search for **"HTML Content"** by Nova Silva
3. Click **Add** and accept permissions

## Step 2 — Set Up Your Data Model

Your model needs these tables (column names are examples — adapt to yours):

```
fact_production
  ├── agent_id (FK)
  ├── unit_id  (FK)
  ├── amount
  └── target_amount

fact_collections
  ├── unit_id  (FK)
  ├── date_id  (FK)
  ├── billed_amount
  └── collected_amount

dim_agent
  ├── agent_id (PK)
  ├── agent_name
  ├── email
  ├── role
  └── unit_name

dim_unit
  ├── unit_name (PK)
  └── unit_manager_email

dim_date
  ├── date_id (PK)
  ├── month_num
  ├── year_num
  ├── is_current_month (calculated)
  └── is_prev_month    (calculated)
```

## Step 3 — Import a DAX Measure

1. Open Power BI Desktop → select any table in the Fields pane
2. Click **New Measure** in the ribbon
3. Paste the full content of the `.dax` file
4. Rename the measure if needed
5. Place an **HTML Content** visual on the canvas
6. Drag the measure into the **"HTML Content"** field well

## Step 4 — Adapt Table/Column Names

Each `.dax` file uses generic names like `fact_production[amount]`. Replace with your actual table and column names.

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Visual shows raw HTML text | Measure must return a text string — check for DAX syntax errors |
| Charts don't render | CDN blocked by network policy — download Chart.js and host internally |
| RLS not filtering | Check that UPN in table matches exact login email in Power BI Service |
| `LOOKUPVALUE` returns blank | Ensure relationship exists between tables or use explicit CALCULATE filter |
