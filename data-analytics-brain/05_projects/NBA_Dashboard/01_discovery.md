# 🧭 Phase 1 — Discovery Summary: NBA Dashboard

> **Project:** NBA Team & Conference Analytics Dashboard
> **Client:** Website owner — NBA analytics/fan site
> **Date:** 2026-04-22
> **Status:** ✅ Complete

---

## Dashboard Purpose (one sentence)

Track NBA team and conference performance across multiple seasons, surfacing both standard standings and deeper home/away insights — embedded in a public-facing website for fans, analysts, and recruiters.

---

## Audience

| Audience | Technical Level | Primary Need |
|---|---|---|
| NBA fans | Low–medium | Quick standings, interesting insights, clean UI |
| Data analysts (learning) | Medium–high | Methodology, deeper metrics, patterns |
| HR / recruiters | Low | Portfolio-quality presentation, visual polish |

> Key implication: the dashboard must work at two levels — **at a glance** for fans, and **on closer inspection** for analysts. Presentation and UI quality are first-class requirements.

---

## Data Sources

- **Format:** Excel / CSV files
- **Scope:** Multiple NBA seasons (multi-season trend analysis is possible)
- **Grain:** Team-level and conference-level (no individual player data at this stage)

---

## What the dashboard must answer

### Must Have
- Conference standings (current + historical)
- Win/loss record by team
- Home vs away win rate by team
- Which teams perform significantly better or worse at home
- Point differential: home vs away per team

### Should Have
- Head-to-head home/away trends (e.g., Team A at home vs Team B historically)
- Rolling form / win streak trends
- Conference-level aggregates (East vs West)

### Nice to Have
- Season-over-season trend for home advantage
- Matchup explorer (filter by two teams)
- Highlight: "biggest home underperformers" and "best road teams"

---

## Design & UI Requirements

- **Delivery format:** Embedded in a website (Power BI Embed)
- **Color palette:** Light, warm, editorial — sand, cream, parchment tones
- **Aesthetic reference:** Think The Athletic / premium sports editorial (not typical dark sports dashboard)
- **Priority:** High — UI polish and presentation quality are a primary deliverable

---

## Constraints

- No individual player data
- ASAP delivery (MVP this week)
- Embedded via Power BI Service (embed token or publish-to-web)

---

## Next Step → [[02_data_sources]]
