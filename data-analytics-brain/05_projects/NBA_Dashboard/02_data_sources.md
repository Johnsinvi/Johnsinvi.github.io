# 🗄️ Phase 2 — Data Sources: NBA Dashboard

> **Status:** ✅ Complete — files cleaned and saved
> **Date:** 2026-04-22

---

## Source Files

| File | URL | Format | Rows | Columns |
|---|---|---|---|---|
| game_log.csv | GitHub raw (client repo) | CSV | 1,329 raw → 1,322 clean | 8 |
| team_standings.csv | GitHub raw (client repo) | CSV | 30 | 16 (4 useful) |

---

## Data Assessment

### game_log.csv

**Grain:** One row = one NBA game played (home + away result)

**Date range:** 2025-10-02 → 2026-04-21 (full 2025-26 regular season)

**Season:** Single season (2025-26) — multi-season data not present despite client mention; dashboard will reflect current season only.

**Issues found & resolved:**

| Issue | Detail | Resolution |
|---|---|---|
| Non-NBA rows | 7 rows with game_id prefix `325` — All-Star/international exhibition games with scores ~20-48 pts | **Removed** — not relevant to team analysis |
| Abbreviation mismatch | game_log uses GSW, NYK, NOP, UTA, WAS, SAS — standings uses GS, NY, NO, UTAH, WSH, SA | **Standardized** via mapping in Power Query |
| High scores (>149 pts) | 11 games — all valid NBA games (fast-paced matchups, not data errors) | **Kept** — legitimate data |
| Date stored as text | Date column arrives as object type | **Cast to Date** in Power Query |

**No nulls. No duplicate game_ids.**

**Overall home win rate:** 55.5% (healthy NBA baseline — model is sound)

---

### team_standings.csv

**Grain:** One row = one NBA team (30 teams, current season standings)

**Columns kept:** team_id, Team (abbreviation), Team Name, Conference

**Columns dropped:** Seed, Wins, Losses, Win%, Record, Streak, Home Record, Away Record, Last 10, Pts/Game, Opp Pts/Game, Point Diff

> Note: The dropped columns overlap heavily with what we'll calculate from game_log. Computing from raw game data is more accurate and flexible — we'll derive our own standings and stats.

---

## Cleaned Files (saved to project folder)

- `game_log_clean.csv` — 1,322 rows, 13 columns (includes derived: Home Win, Away Win, Point Diff, Season, Month)
- `team_standings_clean.csv` — 30 rows, 4 columns

---

## Derived Columns Added (Power Query / pre-processing)

| Column | Logic |
|---|---|
| Home Win | 1 if Home Score > Away Score, else 0 |
| Away Win | 1 if Away Score > Home Score, else 0 |
| Point Diff | Home Score − Away Score |
| Season | NBA season label (Oct → next year Jun logic) |
| Month | Year-Month period for trend charts |

---

## Data Delivery & Refresh

| Attribute | Value |
|---|---|
| Delivery method | GitHub raw URL (direct connection) |
| Format | CSV |
| Frequency | Static for now (end-of-season snapshot) |
| Owner | Client (Johnsinvi GitHub repo) |
| Refresh schedule | Manual re-import if client updates files |
| Mode | Import Mode (data fits in memory, no real-time requirement) |

---

## Checklist

- [x] Connected to both data sources successfully
- [x] Identified grain of each table
- [x] Assessed and resolved null rates
- [x] Fixed data type issues (dates)
- [x] Removed non-NBA exhibition rows
- [x] Standardized team abbreviations across both tables
- [x] Renamed/selected columns to clean, usable names
- [x] Documented source, format, frequency, and owner
- [x] Confirmed Import Mode appropriate

---

## Next Step → [[03_data_model]]
