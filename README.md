# Bálggis

**Bálggis** means "path" or "trail" in Northern Sami — a name that reflects the tool's purpose: mapping the paths and connections within an organization.

A self-contained strategic mapping tool that runs directly in the browser with no installation, server, or login required.

## Getting started

1. Download or clone this repository
2. Open `index.html` in Chrome, Firefox, or Edge
3. Done — no installation needed

Data is stored locally in the browser (`localStorage`). Back up regularly via **File ▾ → Export JSON**.

---

## Features

### Views
| View | Description |
|---|---|
| **Map** | Graphical view with nodes and relation lines. Drag, connect, and arrange freely on an infinite canvas. |
| **Table** | Structured table view per category with inline editing of all parameter values. |
| **Matrix** | Cross-table between two categories — view and edit relations as dots in a grid. |
| **Timeline** | Gantt-style view for nodes with date parameters. |
| **Dashboard** | Customizable control panel with widgets from various categories and objects. |

### Data model
- **Categories** — groupings of objects (e.g. Strategic goal, KPI, Capability, Initiative, IT system)
- **Objects** — nodes within a category, with custom parameter values
- **Parameters** — field definitions per category:
  - `text`, `number`, `date`, `yes/no`
  - `status` — traffic light (red/yellow/green), with optional auto-aggregation from child nodes
  - `select`, `multi-select`
  - `tags` — freeform chip-based tag input (Enter or comma to add)
  - `calculated` — formula using `{ParamName}` references and math functions
- **Relations** — links between objects: hierarchical, measures, supports, delivers to
- **Perspectives** — filter views for different audiences (board, CIO, operations team)

### Dashboard widgets
- KPI / single value (with optional progress bar toward target)
- Object count
- Status distribution (traffic light breakdown)
- Progress indicator
- Object list (mini-table)
- Text / heading
- Deviation list (objects triggering conditional formatting rules)

### Other features
- **Group boxes** — visual swim lanes on the map canvas
- **Conditional formatting** — automatic coloring of nodes and table rows based on parameter values
- **Perspective filtering** — tailored views for different audiences
- **Auto-layout** — automatic node placement on the map
- **Undo / Redo** — full support (Ctrl+Z / Ctrl+Shift+Z), up to 20 steps
- **Line style** — curved bezier or right-angle (orthogonal) relation lines
- **Node styles** — 7 visual styles: flat, card, minimal, filled, outlined, tinted, duo
- **Norwegian / English UI** — switch language in Settings → General
- **Storage warning** — alerts when localStorage usage exceeds 3.5 MB of the ~5 MB browser limit

### Import and export
| Action | Description |
|---|---|
| **Import JSON** | Full restore of all data |
| **Import CSV** | Add objects to a category; map columns manually |
| **Export JSON** | Full backup of all data |
| **Export for AI** | Clean JSON structured for LLM analysis |
| **Map as SVG** | Vector export of the map view |
| **Map as PNG** | Raster export of the map view |
| **HTML report** | Unified report covering all views — tables, matrix, timeline and dashboard snapshot. Respects the active perspective. |

---

## Files

| File | Description |
|---|---|
| `index.html` | The entire application — one self-contained HTML file |
| `hjelp.html` | Full user guide in Norwegian |
| `help.html` | Full user guide in English |

---

## Technical

- **Pure vanilla JS + CSS** — no dependencies, no build step
- **Single-file architecture** — everything in `index.html`: CSS, HTML shell, JavaScript
- **Global state** `S` — all mutations call `save()` (localStorage) then `render()`
- **localStorage key:** `nx2`
- **i18n:** `STRINGS` object + `t(key)` function; language stored in `S.settings.lang`

### Reset to demo data
Open DevTools → Console and run:
```js
localStorage.removeItem('nx2'); location.reload();
```

### Syntax check after edits
```bash
node -e "
const fs = require('fs');
const html = fs.readFileSync('index.html','utf8');
const match = html.match(/<script>([\s\S]*?)<\/script>/);
new Function(match[1]);
console.log('JS syntax OK');
"
```

---

## About the name

*Bálggis* is a Northern Sami word meaning "path" or "trail". The name reflects the tool's core purpose: charting the paths between goals, capabilities, systems, and initiatives in an organization.

---

## License

Private use. Not for redistribution without permission.
