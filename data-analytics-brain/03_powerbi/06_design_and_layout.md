# 🎨 Phase 6 — Design & Layout

> **Goal:** Plan the visual language of the dashboard — layout grid, visual hierarchy, color palette, typography, and interactivity — before touching Power BI.

← Back to [[03_powerbi/05_kpis_and_metrics]]
Next → [[03_powerbi/07_build_and_interactivity]]

---

## Design the layout before opening Power BI

Sketch the page layout before touching Power BI. Decide:

- How many pages does this report have?
- What is the name and purpose of each page?
- What lives at the top (summary), middle (breakdown), bottom (detail)?
- Where do filters/slicers go?

> Tip: Use a 12-column grid mentally. Think in rows: KPI cards row → main chart → supporting charts → table.

### Using an AI design tool for wireframing

Tools like Claude Artifacts or similar AI design generators can produce quick interactive HTML wireframes from a plain-language brief. This is useful for aligning with a client before building anything.

**How to use effectively:**
1. Describe the dashboard goal, audience, page structure, and data metrics in plain language
2. Specify the canvas size (e.g., 1280×720px) and style direction (colors, tone)
3. Ask specifically for **Power BI layout only** — not a full website — to avoid getting a full webpage design with headers, footers, and sidebars
4. Export as a standalone HTML file and share with the client for review
5. Treat the output as a rough reference, not a final spec — adapt visual positions and sizing when building for real in Power BI

**Key prompt elements that help:**
- "Power BI report page, not a website"
- "Canvas: 1280×720px"
- "Show placeholder visuals in their approximate positions"
- "Use these exact colors: [hex codes]"
- "Pages: [list with names and main visuals on each]"

> The wireframe is for layout direction only. Real visual types, data connections, and formatting happen in Power BI.

---

## Choosing the right visual

| Question type | Best visual | Avoid |
|---------------|-------------|-------|
| How much total? | KPI Card, Big Number | Pie chart |
| Trend over time | Line chart, Area chart | 3D charts |
| Comparison between categories | Bar / Column chart | Pie or donut |
| Part of a whole (few categories) | Donut chart, Stacked bar | Pie with many slices |
| Ranking | Horizontal bar chart | Tables for rankings |
| Geographic distribution | Map, Filled map | Bar chart for geo |
| Correlation / distribution | Scatter plot | Line chart |
| Detailed data | Table, Matrix | Multiple KPI cards |
| KPI vs target | Gauge, Bullet chart, KPI visual | Plain number |
| Progress | Progress bar (custom), KPI | Gauge (too much space) |

> **Rule:** Use the simplest visual that honestly communicates the data. Never use 3D. Rarely use pie charts (3+ categories → use bar instead).

---

## Layout hierarchy

Good dashboards follow a reading pattern — top-left to bottom-right, most important to least:

```
┌─────────────────────────────────────────────────────┐
│  LOGO / TITLE           PAGE NAVIGATION    FILTERS  │
├──────────┬──────────┬──────────┬───────────────────┤
│  KPI 1   │  KPI 2   │  KPI 3   │     KPI 4         │
├──────────┴──────────┴──────────┴───────────────────┤
│                   Main Chart (line/bar)             │
├─────────────────────────┬───────────────────────────┤
│   Supporting Chart 1    │   Supporting Chart 2      │
├─────────────────────────┴───────────────────────────┤
│                   Detail Table (optional)           │
└─────────────────────────────────────────────────────┘
```

---

## Color guidelines

- **Limit the palette** — 2 main colors + 1 accent + neutrals (gray, white, dark)
- **Use color with meaning** — not for decoration. Green = good/positive, Red = bad/negative, Gray = inactive/comparison
- **Maintain contrast** — text must be readable on all backgrounds (WCAG AA: 4.5:1 ratio minimum)
- **Consistent color per category** — if "North" is blue on one chart, it must be blue on all charts

### Suggested neutral professional palette
```
Primary:    #2563EB  (blue)
Secondary:  #64748B  (slate gray)
Accent:     #F59E0B  (amber — for highlights)
Background: #F8FAFC  (light gray-white)
Card bg:    #FFFFFF
Text:       #1E293B
Positive:   #16A34A  (green)
Negative:   #DC2626  (red)
```

---

## Typography & sizing

- Use a single font family (Segoe UI is clean and native to Power BI)
- **Title text:** 14–16pt bold
- **KPI value:** 24–36pt bold
- **Chart labels:** 10–12pt
- **Axis labels:** 9–11pt
- **Table text:** 10–11pt
- Align consistently — don't mix left and center alignment on the same page

---

## Spacing and alignment

- Use the Power BI alignment tools (Format → Align) — never eyeball it
- All cards in a row must be the same height
- Leave breathing room (padding) inside cards — at least 8–12px equivalent
- Group related visuals visually with subtle background shapes/containers

---

## Interactivity planning

Before building, decide:
- Which visuals should cross-filter others when clicked?
- Which visuals should NOT filter others? (Use "Edit Interactions" to disable)
- Are there drill-throughs to a detail page?
- Are there tooltips pages (custom tooltip when hovering a visual)?
- Are there page navigation buttons?
- Are there bookmarks for switching views (e.g., chart ↔ table toggle)?

Document each interactive behavior so you know what to build in Phase 7.

---

## Checklist

- [ ] Sketched layout for each page on paper or in Figma/Miro
- [ ] Named and defined each page (purpose, audience, main question)
- [ ] Selected the visual type for each metric (from Phase 5)
- [ ] Defined color palette and documented hex codes
- [ ] Decided typography sizes for title, KPI, chart, table
- [ ] Planned slicer/filter placement
- [ ] Documented all interactive behaviors (cross-filter, drill-through, bookmarks)
- [ ] Identified any custom tooltip pages needed

---

## Related

- [[03_powerbi/05_kpis_and_metrics]]
- [[03_powerbi/07_build_and_interactivity]]
