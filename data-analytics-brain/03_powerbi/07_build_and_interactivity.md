# ⚙️ Phase 7 — Build & Interactivity

> **Goal:** Bring the design to life in Power BI — create all visuals, wire up interactions, and build the navigation and UX tricks that make the report feel polished and intuitive.

← Back to [[03_powerbi/06_design_and_layout]]
Back to start → [[03_powerbi/Power BI - Map of Content]]

---

## Working in PBIR format — visual authoring

When working with a `.pbip` project, each visual on each page is stored as its own `visual.json` file. Understanding this format lets you audit, copy, and bulk-author visuals in code — but there are strict rules.

### The golden rule: let Power BI Desktop write the first version

**Do not try to hand-author visual.json files from scratch.** The schema version, SourceRef format, and property structure must match exactly what the running version of Power BI Desktop produces. The safest workflow is:

1. Open the `.pbip` with **bare minimum visuals** (just `visualType` and `position`)
2. In Desktop, drag one field onto one visual and save (`Ctrl+S`)
3. Read that saved `visual.json` — it is the ground-truth format for your version
4. Use it as the template to script all remaining visuals

### Bare minimum visual.json (always opens)

```json
{
  "$schema": "https://developer.microsoft.com/json-schemas/fabric/item/report/definition/visualContainer/2.8.0/schema.json",
  "name": "visual_name",
  "position": { "x": 0, "y": 0, "height": 300, "width": 400, "tabOrder": 0 },
  "visual": {
    "visualType": "barChart"
  }
}
```

> Note: the schema version is `2.8.0` (not `2.1.0`). Use whatever version Desktop writes — check any saved file to confirm.

### Correct SourceRef format

Power BI Desktop uses the **full table name** directly in `SourceRef` — not short aliases:

```json
"SourceRef": { "Entity": "fact_table_name" }
```

Never use short aliases like `{ "Source": "f" }` — those only work inside `prototypeQuery` which no longer exists in current PBIR versions.

### Full data visual template

```json
{
  "$schema": "https://developer.microsoft.com/json-schemas/fabric/item/report/definition/visualContainer/2.8.0/schema.json",
  "name": "bar_chart_example",
  "position": { "x": 600, "y": 112, "height": 276, "width": 660, "tabOrder": 5 },
  "visual": {
    "visualType": "barChart",
    "query": {
      "queryState": {
        "Category": {
          "projections": [{
            "field": { "Column": { "Expression": { "SourceRef": { "Entity": "dim_table" } }, "Property": "Category Column" } },
            "queryRef": "dim_table.Category Column",
            "nativeQueryRef": "Category Column",
            "active": true
          }]
        },
        "Y": {
          "projections": [{
            "field": { "Measure": { "Expression": { "SourceRef": { "Entity": "fact_table" } }, "Property": "My Measure" } },
            "queryRef": "fact_table.My Measure",
            "nativeQueryRef": "My Measure"
          }]
        }
      },
      "sortDefinition": {
        "sort": [{ "field": { "Measure": { "Expression": { "SourceRef": { "Entity": "fact_table" } }, "Property": "My Measure" } }, "direction": "Descending" }],
        "isDefaultSort": true
      }
    }
  }
}
```

**Key points:**
- `Column` → for dimension columns; `Measure` → for calculated measures
- `queryRef` = `"TableName.ColumnName"` (full path)
- `nativeQueryRef` = just the column/measure name
- `active: true` on the primary category projection
- `isDefaultSort: true` for auto-sort, `false` for user-set sort

### Textbox (title) format

```json
"visual": {
  "visualType": "textbox",
  "objects": {
    "general": [{
      "properties": {
        "paragraphs": [{
          "textRuns": [{ "value": "Your title text here" }]
        }]
      }
    }]
  }
}
```

### Action button (page navigation) format

```json
"visual": {
  "visualType": "actionButton",
  "objects": {
    "text": [
      { "properties": { "show": { "expr": { "Literal": { "Value": "true" } } } } },
      { "properties": { "text": { "expr": { "Literal": { "Value": "'Button Label'" } } } }, "selector": { "id": "default" } }
    ]
  },
  "visualContainerObjects": {
    "visualLink": [{
      "properties": {
        "show": { "expr": { "Literal": { "Value": "true" } } },
        "type": { "expr": { "Literal": { "Value": "'PageNavigation'" } } },
        "navigationSection": { "expr": { "Literal": { "Value": "'target_page_id'" } } }
      }
    }]
  }
}
```

> For the button that represents the **current page**, omit `navigationSection` — Power BI won't let you navigate to the page you're already on.

### What belongs where in the visual object

| Property | Location | Notes |
|----------|----------|-------|
| `query` / `queryState` | `visual.query` | Data bindings only |
| Visual-specific formatting (dataLabels, legend, etc.) | `visual.objects` | Chart-level properties |
| Container formatting (title, background, border) | `visual.visualContainerObjects` | Wraps the visual |
| Page navigation | `visual.visualContainerObjects.visualLink` | Action buttons only |

**`prototypeQuery` does not exist in current PBIR** — remove it if it appears. All data resolution happens through `queryState` alone.

### Slicer role name

Slicers use `"Field"` (capital F) as the queryState role name:

```json
"queryState": {
  "Field": {
    "projections": [{ ... }]
  }
}
```

### Troubleshooting: "Failed to load the report"

This error (with an Activity ID) means the report renderer crashed — different from schema validation errors (which list individual issues). Common causes:

- Wrong schema version (`2.1.0` instead of `2.8.0`)
- `SourceRef` using aliases (`"Source": "d"`) instead of full entity names (`"Entity": "dim_teams"`)
- Invalid properties in `visual.objects` (e.g., nested arrays where flat objects are expected)
- Textbox with a custom paragraphs format that the renderer can't parse

**Isolation approach:** Strip all visuals to bare minimum (just `visualType`). If it opens → the crash is in visual content. If it still crashes → the issue is in the semantic model or `report.json`.

---

## Build order — follow this sequence

1. Set up the **canvas size** and **background** first (before any visuals)
2. Place the **header** (title, logo, navigation bar shape)
3. Drop in **KPI cards** across the top
4. Add the **main chart(s)**
5. Add **supporting charts**
6. Add **slicers and filters**
7. Add a **detail table** if needed
8. Wire up **interactions** (edit interactions, cross-filter behavior)
9. Build **bookmarks** for toggle views
10. Build **navigation buttons** and page links
11. Build **drill-through pages**
12. Build **tooltip pages**
13. Final polish — alignment, spacing, font sizes, colors

---

## Canvas setup

- `View → Page View → Actual Size` for accurate sizing
- Standard canvas: 1280 × 720 px (widescreen) or 1920 × 1080 px
- For mobile: create a separate phone layout (`View → Mobile Layout`)
- Set background color in `Format Page → Page background`

---

## Building visuals

For each visual:
1. Add the visual from the Visualizations pane
2. Drop in the correct measure (Y-axis / Value) and dimension (X-axis / Axis / Legend)
3. Format: turn off unnecessary elements (gridlines, default titles if you're adding your own)
4. Set data colors explicitly — never rely on defaults
5. Turn on data labels only if they add clarity (not on dense line charts)
6. Set a descriptive visual title that states what it shows, not just "Sales"

---

## Edit Interactions — control what filters what

By default, every visual cross-filters all others. Control this:

1. Click a visual
2. Go to `Format → Edit Interactions`
3. For each other visual, choose: Filter 🔽 | Highlight 🔆 | None ⊘
4. Disable interaction from a slicer to a visual if that visual should be unaffected

> Common pattern: let a date slicer filter all visuals, but prevent a "Total Company" KPI card from being filtered by a product slicer.

---

## Bookmarks — toggle between views

Bookmarks save the current state of a page (filters, visibility, slicer values). Use them to:

- Toggle between a **chart view** and a **table view** of the same data
- Show/hide a filter panel
- Reset all filters to default state

**How to create a toggle:**
1. Create two versions of a section (e.g., a chart and a table, overlapping)
2. Show one, hide the other → `Bookmarks → Add → "Chart View"`
3. Swap visibility → `Bookmarks → Add → "Table View"`
4. Add two buttons, assign each bookmark via `Action → Bookmark`
5. Turn off `Data` in bookmark settings if you don't want it to capture filter state

---

## Buttons and navigation

### Page navigation buttons
1. Insert → Button → Navigator → Page Navigator (auto-creates for all pages)
   — OR —
2. Insert → Button → Blank → Format → Action → Type: Page Navigation → Destination: [page name]

### Back buttons (for drill-through pages)
- Insert → Button → Back — this is built in, no configuration needed
- Place it top-left of the drill-through page

### Icon buttons
- Use blank buttons with an image or icon set as Fill
- Or use the built-in button types (Arrow, Information, Q&A, etc.)
- Format the button states: Default / Hover / Pressed — always set all three

---

## Drill-through pages

Drill-through lets users right-click a data point and go to a detail page filtered to that item.

**Setup:**
1. Create a new page for the detail view (e.g., "Product Detail")
2. In the Visualizations pane on that page, drag the field you'll drill on (e.g., ProductName) into the **Drill through** well
3. Build the detail visuals — they'll auto-filter to the selected item
4. Add a Back button so users can return

---

## Custom tooltip pages

A tooltip page shows when the user hovers over a data point — like a mini pop-up dashboard.

**Setup:**
1. Create a new page → `Format Page → Page information → Allow use as tooltip: ON`
2. Set canvas size to 320 × 240 px (tooltip size)
3. Build a small visual or set of KPI cards on it
4. Go back to the main visual → Format → Tooltip → Type: Report page → Page: [your tooltip page]

---

## Advanced tricks and combinations

### Dynamic title based on slicer
```dax
Dynamic Title = 
"Sales by " & SELECTEDVALUE(DimProduct[Category], "All Categories")
```
Use this measure as the title of a visual via `Format → Title → Value → fx`.

### Conditional formatting
- Use a measure to drive background color of a table cell or KPI card
- `Format → Conditional formatting → Background color → Field value`

### Show/hide a filter panel (bookmark + button trick)
1. Create a rectangle shape as a "panel" with slicers grouped inside
2. Add a bookmark with panel visible ("Open filters")
3. Add a bookmark with panel hidden ("Close filters")
4. Add a button with a filter icon → Action → Bookmark → "Open filters"
5. Add a close (X) button inside the panel → Action → Bookmark → "Close filters"

### Sync slicers across pages
`View → Sync Slicers` → select which pages share the same slicer state

### What-if parameter slicer
Created in Phase 4 — just drop the auto-generated slicer onto the canvas and connect to your DAX measure

---

## Checklist

- [ ] Canvas size and background set
- [ ] Header (title, logo, navigation bar) placed
- [ ] All KPI cards built and formatted
- [ ] All charts built with correct measures and dimensions
- [ ] Data colors set explicitly on all visuals
- [ ] Visual titles are descriptive (not just "Sum of Sales")
- [ ] Edit Interactions configured — no unintended cross-filtering
- [ ] Bookmarks created for all toggle views
- [ ] All buttons have all three states styled (Default, Hover, Pressed)
- [ ] Page navigation buttons working
- [ ] Drill-through pages built with Back buttons
- [ ] Custom tooltip pages built and assigned
- [ ] Slicers synced across pages where needed
- [ ] Mobile layout created (if required)
- [ ] Tested on full screen — everything aligned, nothing cropped
- [ ] Shared to Power BI Service and tested with actual user account

---

## Related

- [[03_powerbi/06_design_and_layout]]
- [[03_powerbi/Power BI - Map of Content]]
