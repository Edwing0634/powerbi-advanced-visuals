# DAX Patterns Reference

## Pattern 1 — Semaphore Color Switch

```dax
VAR _color =
    SWITCH(
        TRUE(),
        [Achievement] >= 100, "#28a745",   -- green
        [Achievement] >=  80, "#ffc107",   -- yellow
        "#dc3545"                           -- red
    )
```

## Pattern 2 — CONCATENATEX HTML Table Rows

```dax
VAR _rows =
    CONCATENATEX(
        TOPN( 10, _dataset, [SortColumn], DESC ),
        "<tr><td>" & [Column1] & "</td><td>" & [Column2] & "</td></tr>",
        "",           -- no separator between rows
        [SortColumn], DESC
    )
```

## Pattern 3 — Aliases Without AS

```dax
-- Correct (no AS keyword)
SUMMARIZE(
    fact_production,
    dim_agent[agent_name],
    "Prod",   SUM( fact_production[amount] ),
    "Target", SUM( fact_production[target_amount] )
)

-- Wrong
SUMMARIZE(
    fact_production,
    dim_agent[agent_name] AS AgentName,   -- syntax error
    ...
)
```

## Pattern 4 — Previous Month Filter

```dax
VAR _cur_month  = MONTH( TODAY() )
VAR _cur_year   = YEAR( TODAY() )
VAR _prev_month = IF( _cur_month = 1, 12, _cur_month - 1 )
VAR _prev_year  = IF( _cur_month = 1, _cur_year - 1, _cur_year )

VAR _prev_data =
    CALCULATE(
        [YourMeasure],
        dim_date[month_num] = _prev_month,
        dim_date[year_num]  = _prev_year
    )
```

## Pattern 5 — Calibration Offset (+N percentage points)

Use when your data source has a known systematic difference vs. an official system:

```dax
VAR _raw_achievement  = DIVIDE( [Production], [Target], 0 ) * 100
VAR _adj_achievement  = _raw_achievement + 4   -- +4pp calibration
```

Document the offset reason in a comment so future maintainers understand it.

## Pattern 6 — RLS-Aware Measure

```dax
-- Works correctly under RLS because CALCULATE respects filter context
VAR _user_production =
    CALCULATE(
        SUM( fact_production[amount] ),
        ALLEXCEPT( dim_agent, dim_agent[unit_name] )
    )
-- dim_agent is already filtered by RLS before this measure runs
```

## Pattern 7 — Responsive Number Formatting

```dax
VAR _label =
    SWITCH(
        TRUE(),
        _value >= 1000000000, FORMAT( _value / 1000000000, "#,##0.0" ) & "B",
        _value >= 1000000,    FORMAT( _value / 1000000,    "#,##0.0" ) & "M",
        _value >= 1000,       FORMAT( _value / 1000,       "#,##0.0" ) & "K",
        FORMAT( _value, "#,##0" )
    )
```
