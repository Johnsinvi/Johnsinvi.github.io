# 🧩 Phase 3 — Data Model: NBA Dashboard

> **Status:** ✅ Complete — TMDL files written to pbir project
> **Date:** 2026-04-22

---

## Model Architecture

Star schema: 1 fact table + 2 dimension tables + 1 calendar table.

```
dim_calendar ──────────────────────────┐
  (Date)                               │  Date → Date
                                       ▼
                              fact_game_results
  (team_id → Team)            (grain: 1 row per team per game)
                                       ▲
dim_teams ─────────────────────────────┘
  (Team)
```

---

## Tables

### fact_game_results
**Grain:** One row per team per game (unpivoted from wide format)
**Rows:** ~2,644 (2 rows per game × 1,322 games)

| Column | Type | Description |
|---|---|---|
| game_id | Integer | Unique game identifier |
| Date | Date | Game date |
| team_id | Text | Team abbreviation (FK → dim_teams[Team]) |
| Team Name | Text | Short team name (e.g., "Celtics") |
| Points Scored | Integer | Points this team scored |
| Points Allowed | Integer | Points opponent scored |
| Win | Integer | 1 = win, 0 = loss |
| Point Diff | Integer | Points Scored − Points Allowed |
| Location | Text | "Home" or "Away" |
| Season | Text | NBA season label (e.g., "2025-26") |
| Month | Text | Year-Month string (e.g., "2025-10") |

### dim_teams
**Grain:** One row per NBA team (30 rows)

| Column | Type | Description |
|---|---|---|
| team_id | Integer | Numeric ID (not used for join) |
| Team | Text | Abbreviation — **primary key** used for relationship |
| Team Name | Text | Full team name |
| Conference | Text | "Eastern Conference" or "Western Conference" |

### dim_calendar
**Grain:** One row per calendar day (Oct 1 2025 → Apr 30 2026)

| Column | Type | Description |
|---|---|---|
| Date | Date | **Primary key** |
| Year | Integer | Calendar year |
| Month Number | Integer | 1–12 |
| Month Name | Text | Full month name |
| Month Short | Text | 3-letter abbreviation |
| Month Year | Text | e.g., "Oct 2025" |
| Day of Week | Text | Full day name |
| Season | Text | "2025-26" |

---

## Relationships

| From | To | Cardinality | Direction |
|---|---|---|---|
| fact_game_results[team_id] | dim_teams[Team] | Many → One | Single |
| fact_game_results[Date] | dim_calendar[Date] | Many → One | Single |

---

## DAX Measures (defined on fact_game_results)

| Measure | Description |
|---|---|
| Games | COUNTROWS of fact |
| Wins | SUM of Win column |
| Losses | Games − Wins |
| Win% | Wins / Games × 100 |
| Avg Points Scored | Average points per game |
| Avg Points Allowed | Average points allowed per game |
| Avg Point Diff | Average margin per game |
| Home Win% | Win% filtered to Location = "Home" |
| Away Win% | Win% filtered to Location = "Away" |
| Home Advantage | Home Win% − Away Win% |
| Avg Home Point Diff | Avg margin at home |
| Avg Away Point Diff | Avg margin away |
| Home Games | Count of home appearances |
| Away Games | Count of away appearances |
| Home Wins | Count of home wins |
| Away Wins | Count of away wins |

---

## Power Query Transformations (applied in M)

All cleaning happens in the fact_game_results M query:
1. Promote headers + set correct types
2. Remove exhibition games (game_id prefix `325`)
3. Standardize abbreviations (GSW→GS, NYK→NY, NOP→NO, UTA→UTAH, WAS→WSH)
4. Unpivot: split each game into 2 rows (Home + Away perspective)
5. Add Win flag, Point Diff, Season, Month columns

---

## Next Step → [[04_dax_calculations]]
