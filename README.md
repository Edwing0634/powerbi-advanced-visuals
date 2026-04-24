# 📊 Power BI Advanced Visuals

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Advanced-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Last Commit](https://img.shields.io/github/last-commit/Edwing0634/powerbi-advanced-visuals)

> A production-grade collection of **DAX measures** that render interactive HTML/CSS/JS visuals inside Power BI — including Chart.js charts, semaphore KPI cards, ranked tables with CONCATENATEX, 3D force graphs, and a complete Row-Level Security setup for a 4-level sales hierarchy.

---

## ✨ Features

- **Semaphore KPI Cards** — dynamic color-coded cards with calibration offset support (+N pp adjustments)
- **Ranked HTML Tables** — CONCATENATEX-built tables with medals, badges, and delta indicators vs. previous month
- **Collections Tracker** — dual-panel card (current month vs. previous month close) with unit breakdown
- **Chart.js Bar & Gauge** — fully embedded charts rendered from DAX with CDN-loaded Chart.js
- **3D Force Graph** — interactive organizational hierarchy visualization using vasturiano/3d-force-graph
- **4-Level RLS** — complete Row-Level Security setup: Admin → Senior → GA → GU with USERPRINCIPALNAME()
- **Design System** — consistent CSS tokens and reusable HTML templates documented in `/templates`

---

## 🛠 Tech Stack

| Component | Technology |
|-----------|-----------|
| BI Platform | Power BI Desktop + Service |
| Measure language | DAX |
| HTML Visual | HTML Content by Nova Silva (AppSource) |
| Charts | Chart.js 4.4 (CDN) |
| 3D Graph | vasturiano/3d-force-graph 1.73 (CDN) |
| Security | Power BI RLS + USERPRINCIPALNAME() |

---

## ⚡ Quick Start

### 1. Install the HTML Content Visual

In Power BI Desktop → Visualizations pane → `...` → **Get more visuals** → search **"HTML Content"** (by Nova Silva).

### 2. Import a measure

```
1. Open Power BI Desktop
2. Select any table in the Fields pane
3. New Measure → paste the full .dax file content
4. Place an HTML Content visual on the canvas
5. Drag the measure into the "HTML Content" field well
```

### 3. Adapt column names

Each `.dax` file uses generic table/column names. Replace with your model's actual names:

```dax
-- Generic (in this repo)
SUM( fact_production[amount] )

-- Your model
SUM( YourTable[YourColumn] )
```

See [Installation Guide](docs/installation_guide.md) for full data model requirements.

---

## 📁 Project Structure

```
powerbi-advanced-visuals/
├── measures/
│   ├── html-kpi-cards/
│   │   ├── semaphore_card.dax          # Production KPI with traffic-light color
│   │   └── collections_semaphore.dax   # Dual-panel collections card
│   ├── html-ranked-tables/
│   │   ├── top10_ranking.dax           # CONCATENATEX leaderboard with medals
│   │   └── collections_table.dax       # Unit-level collections breakdown
│   ├── chartjs-embedded/
│   │   ├── bar_chart_measure.dax       # Top-5 units bar chart
│   │   └── gauge_measure.dax           # Half-doughnut achievement gauge
│   └── 3d-force-graph/
│       └── force_graph_measure.dax     # Interactive org hierarchy graph
├── rls/
│   ├── role_definitions.dax            # DAX filters per role
│   └── rls_setup_guide.md             # Step-by-step RLS configuration
├── templates/
│   ├── html_template_base.html         # Reference HTML structure
│   └── css_variables.md               # Design tokens for DAX strings
└── docs/
    ├── installation_guide.md
    └── dax_patterns.md                 # Reusable DAX patterns reference
```

---

## 🏗 Architecture

```
Power BI Data Model
        │
        ▼
DAX Measure (returns TEXT string)
        │
        ▼
HTML Content Visual (sandboxed iframe)
  ├── Inline CSS styles
  ├── CDN JavaScript (Chart.js / 3d-force-graph)
  └── Dynamic values injected via DAX string concatenation
```

---

## 🔐 RLS Hierarchy

```
Admin           → sees all agents, all units, all data
  └── Senior    → sees all units, agents under their supervision
        └── GA  → sees only their assigned unit
              └── GU → sees only their assigned group
```

Each level uses `USERPRINCIPALNAME()` to filter `dim_agent` and `dim_unit`.  
See [`rls/role_definitions.dax`](rls/role_definitions.dax) and [`rls/rls_setup_guide.md`](rls/rls_setup_guide.md).

---

## 💡 Key DAX Patterns

```dax
-- Semaphore color (no AS keyword in aliases)
VAR _color = SWITCH( TRUE(),
    [Achievement] >= 100, "#28a745",
    [Achievement] >=  80, "#ffc107",
    "#dc3545"
)

-- CONCATENATEX ranked table
VAR _rows = CONCATENATEX(
    TOPN( 10, _dataset, [Prod], DESC ),
    "<tr><td>" & [agent_name] & "</td></tr>",
    "",
    [Prod], DESC
)

-- Previous month filter (handles January edge case)
VAR _prev_month = IF( MONTH(TODAY()) = 1, 12, MONTH(TODAY()) - 1 )
VAR _prev_year  = IF( MONTH(TODAY()) = 1, YEAR(TODAY()) - 1, YEAR(TODAY()) )
```

---

## 🤝 Contributing

1. Fork the repository
2. Add your measure in the appropriate `measures/` subfolder
3. Follow the naming convention: `context_visual_description.dax`
4. Include a comment header explaining the measure's purpose and dependencies
5. Open a Pull Request

---

## 📄 License

MIT © [Edwin González](https://github.com/Edwing0634)

---

## 👤 Author

**Edwin González** — Customer Intelligence Specialist & Power BI Developer

[![GitHub](https://img.shields.io/badge/GitHub-Edwing0634-black?logo=github)](https://github.com/Edwing0634)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?logo=linkedin)](https://linkedin.com/in/edwingonzalez)
