# 🎯 Phase 5 — KPIs & Metrics: NBA Dashboard

> **Status:** ✅ Complete
> **Date:** 2026-04-22

---

## What the client asked for vs what the data actually supports

| Client request | Reality in data | Decision |
|---|---|---|
| Conference standings | Fully supported — 30 teams, full season | ✅ Build it |
| Team-level win/loss record | Fully supported | ✅ Build it |
| Home vs away win rate | Fully supported — Location column in fact table | ✅ Build it |
| Home underperformers (wins by smaller margin) | Supported via Avg Home Point Diff vs Avg Away Point Diff | ✅ Build it |
| Multi-season trends | Data is single season only (2025-26) | ⚠️ Flag to client — trend is monthly, not YoY |
| "Any other insights we can suggest" | Found: home advantage ranking, close-game win%, scoring net | ✅ Add as insight pages |

---

## Pages & what each answers

### Page 1 — League Overview
*For fans who want a quick read of the whole season at a glance.*

| Visual | Business question | Metric | Priority |
|---|---|---|---|
| Conference standings table | Where does each team rank? | Win%, Wins, Losses | Must have |
| East vs West win% by month | Is one conference stronger? | Win% by Conference by Month | Must have |
| Top 5 / Bottom 5 teams by Win% | Who's leading, who's struggling? | Win% ranked | Must have |
| Total games played counter | Season context | Games | Should have |

**Key finding from data:** West has been consistently stronger than East (50.6% vs 49.7% overall). East briefly overtook West in Feb and Apr.

---

### Page 2 — Home / Away Performance
*The core insight page — the main reason this dashboard is interesting.*

| Visual | Business question | Metric | Priority |
|---|---|---|---|
| Home Win% vs Away Win% bar chart (all teams) | Who performs better at home? | Home Win%, Away Win% | Must have |
| Home Advantage ranking (sorted bar) | Which teams gain the most from home court? | Home Advantage (Home Win% − Away Win%) | Must have |
| Home Advantage scatter (HPD vs APD) | Do teams win big at home but scrape by away? | Avg Home Point Diff, Avg Away Point Diff | Must have |
| Stat cards: league avg home win% | Context benchmark | Home Win% (all teams) | Should have |

**Key findings from data:**
- League average home win rate: **55.5%** — home court advantage is real
- **Biggest home court beneficiaries:** New Orleans (+21.8pp), Houston (+21.1pp), Miami (+19.2pp), NY Knicks (+18.7pp)
- **Teams that barely benefit from home court:** Charlotte (-1.0pp — actually *better* away), Denver (+3.1pp), Philadelphia (+3.3pp)
- New Orleans is a fascinating case: 43% home win rate but only 21% away — poor team overall, but dramatically worse on the road

---

### Page 3 — Scoring & Margin Insights
*For data analysts and fans who want to go deeper than wins.*

| Visual | Business question | Metric | Priority |
|---|---|---|---|
| Avg Points Scored vs Avg Points Allowed (bubble or bar) | Who dominates offensively and defensively? | Avg Points Scored, Avg Points Allowed | Must have |
| Net Point Differential ranking | Who's the most dominant team by margin? | Avg Point Diff | Must have |
| Close games tracker (≤5 pt margin) | Who thrives or collapses in tight games? | Close Games count, Close Win% | Should have |
| Best road teams (Away Win% sorted) | Who doesn't need a home crowd? | Away Win% | Should have |

**Key findings from data:**
- **Highest scoring:** Denver (121.2 ppg), Miami (120.2), San Antonio (119.7)
- **Best net differential:** OKC (+10.7), San Antonio (+8.5), NY Knicks (+9.15 home point diff)
- **Close game kings:** Orlando Magic win 61.3% of games decided by 5 pts or less — elite clutch performance
- **Close game vulnerability:** Phoenix Suns and Golden State Warriors win under 45% of close games

---

## Measures mapped to visuals

| Measure | Used in |
|---|---|
| Win% | Page 1 standings, Page 1 top/bottom 5 |
| Home Win% | Page 2 bar chart, Page 2 scatter |
| Away Win% | Page 2 bar chart, Page 3 best road teams |
| Home Advantage | Page 2 sorted ranking |
| Avg Home Point Diff | Page 2 scatter |
| Avg Away Point Diff | Page 2 scatter |
| Avg Points Scored | Page 3 scoring visual |
| Avg Points Allowed | Page 3 scoring visual |
| Avg Point Diff | Page 3 net differential |
| Games | Page 1 counter card |
| Wins / Losses | Page 1 standings table |
| Home Games / Away Games | Tooltips |
| Home Wins / Away Wins | Tooltips, Page 2 bar labels |

---

## Slicers / filters (apply across all pages)

- **Conference** — East / West
- **Team** — multi-select
- *(Month slicer deferred — single season, less useful at this stage)*

---

## Next Step → [[06_design_and_layout]]
