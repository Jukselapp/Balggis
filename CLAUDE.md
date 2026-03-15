# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

**Bálggis** — `index.html` is the entire application — a self-contained single-file vanilla JS + CSS tool for strategic organizational mapping. Norwegian UI by default, English available via Settings → General. Open directly in a browser; no build step, no dependencies, no server required.

## Development

No build, lint, or test toolchain. To verify JS syntax after edits:

```bash
node -e "
const fs = require('fs');
const html = fs.readFileSync('index.html','utf8');
const match = html.match(/<script>([\s\S]*?)<\/script>/);
new Function(match[1]);
console.log('JS syntax OK');
"
```

Open in browser to test manually. localStorage key: `'nx2'` — clear in DevTools to reset to seed data.

## Architecture

Everything lives in `index.html`:
- **Lines ~1–545**: CSS (design tokens in `:root`, component styles)
- **Lines ~545–720**: HTML shell (topbar, graph view, table view, side panel)
- **Lines ~720–end**: JavaScript

### State

Global object `S` holds all application data. Every mutation calls `save()` (localStorage) then `render()`.

```js
S = {
  cats, params, nodes, edges,           // core data
  vp,                                    // viewport {z, px, py, snap}
  sel,                                   // selected node id or null
  view,                                  // 'graph'|'table'|'matrix'|'timeline'|'dashboard'
  tblVis, tblDepVis,                    // column visibility in table
  matrixRowCat, matrixColCat,           // matrix view state
  views, activeView,                     // perspectives
  search,                                // search query string
  dashboards, activeDashId,             // dashboard state
  groups,                                // group boxes on canvas
  settings: {
    nodeStyle,     // 'flat'(default)|'card'|'minimal'|'filled'|'outlined'|'tinted'|'duo'
    nodeShadow,    // 'subtle'|'none'|'strong'
    nodeRadius,    // px
    nodeMinW,      // px
    showCatLabel,  // bool
    edgeStyle,     // 'curved'|'orthogonal'
    edgeTypes,     // custom edge type overrides
    canvasBg,      // 'dots'|'grid'|'lines'|'solid'|'none'
    canvasBgColor, // hex
    lang,          // 'no'|'en'
    title,         // project title
  }
}
```

### i18n

`STRINGS` object contains all UI strings for `'no'` and `'en'`. Helper `t(key)` returns the right string based on `S.settings.lang`. `updateI18n()` is called from `render()` and updates DOM elements (view buttons, menu labels, help link, etc.).

### Data model

```
Category → Parameters (1:many, catId FK)
Category → Nodes      (1:many, catId FK)
Node     ↔ Edges      (src/tgt node ids)
Node     → vals       (paramId → value map)
Perspective → catIds  (filter: which categories are visible)
Dashboard → widgets[] (id, type, position, config)
Group     → {x,y,w,h,color,name}  (swim-lane overlays on canvas)
```

Edge `type`: `hierarchy`, `measures`, `supports`, `delivers` (see `EDGE_TYPES`).

Parameter `type`: `text`, `number`, `status`, `boolean`, `date`, `select`, `multiselect`, `tags`, `calc`.

### Rendering pipeline

```
render()
  ├─ updateI18n()        — sync all translated DOM text
  ├─ renderPerspective() — topbar perspective switcher
  ├─ renderGraph()       — applyVP() + renderGroups() + renderNodes() + renderEdges()
  ├─ renderTable()       — category blocks with dep columns
  ├─ renderMatrix()      — cross-table view
  ├─ renderTimeline()    — Gantt-style date view
  ├─ renderDashboard()   — multi-dashboard widget grid
  └─ renderSidePanel()   — node detail or category list
```

During drag/resize, only `moveNodeEl()`/`resizeNodeEl()` + `renderEdges()` are called (performance bypass).

### Node styles (7 active)

`flat` (default), `card`, `minimal`, `filled`, `outlined`, `tinted`, `duo`

Flat and duo use a **header-div** HTML structure (colored band div + white body div) instead of a single card div.

### Key constants

- `PALETTE` / `PALETTE_TEXT` — 12 brand colors with contrast text
- `PARAM_TYPES` — parameter type descriptors
- `EDGE_TYPES` — `{v, l, color, dash, sw, desc}` for each relation type
- `STRINGS` — i18n strings for 'no' and 'en'

### Seed data

`seedDemo()` runs on first load. Creates: Strategisk mål → KPI → Kapabilitet → Initiativ → IT-system, with typed edges and three perspectives.
