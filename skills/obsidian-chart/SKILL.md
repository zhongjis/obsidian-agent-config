---
name: obsidian-chart
description: Create and edit charts in Obsidian using the Charts plugin (Chart.js). Use when the user asks to create bar charts, line charts, pie charts, doughnut charts, radar charts, polar area charts, sankey diagrams, or any data visualization in Obsidian. Also use when modifying existing chart codeblocks, linking charts to markdown tables, integrating charts with Dataview queries, or when the user mentions "chart", "graph", "plot", "visualize data", or refers to Chart.js in an Obsidian context. Even if the user just says "add a chart to this note" or "visualize this data", use this skill.
---

# Obsidian Charts Skill

Create charts in Obsidian using the Charts plugin, which wraps Chart.js. Charts live in fenced codeblocks with YAML syntax. This skill covers chart types, configuration, table linking, Dataview integration, and theming.

## Quick Start

Charts use a ` ```chart ``` ` fenced codeblock with YAML inside:

````yaml
```chart
type: bar
labels: [Monday, Tuesday, Wednesday, Thursday, Friday]
series:
  - title: Hours Worked
    data: [8, 7, 9, 8, 6]
  - title: Hours Slept
    data: [7, 8, 6, 7, 8]
beginAtZero: true
```
````

Every chart needs three things:
- `type` — which chart to render
- `labels` — category labels (x-axis for bar/line, slices for pie, etc.)
- `series` — one or more datasets, each with `title` and `data`

## Chart Types

### Bar Chart
Vertical (default) or horizontal bars. Good for comparing quantities across categories.

````yaml
```chart
type: bar
labels: [Q1, Q2, Q3, Q4]
series:
  - title: Revenue
    data: [120, 150, 180, 200]
  - title: Expenses
    data: [100, 110, 130, 140]
beginAtZero: true
stacked: false
```
````

Set `indexAxis: y` for horizontal bars. Set `stacked: true` to stack series.

### Line Chart
Continuous data over time or ordered categories. Supports fill, smoothing, best-fit lines.

````yaml
```chart
type: line
labels: [Jan, Feb, Mar, Apr, May, Jun]
series:
  - title: Weight (kg)
    data: [82, 80, 79, 78, 77, 76]
tension: 0.3
fill: false
beginAtZero: false
```
````

- `tension` (0–1): 0 = sharp angles, 1 = very smooth curves. 0.3–0.4 is a natural look.
- `fill: true` shades the area under the line.
- `spanGaps: true` connects lines across missing/null data points.
- `bestFit: true` adds a regression line. Use `bestFitTitle` to label it and `bestFitNumber` to pick which series (0-indexed).

### Pie Chart
Proportional slices of a whole. Use `width` to control size — pie charts expand to fill container by default, which can look oversized.

````yaml
```chart
type: pie
labels: [Desktop, Mobile, Tablet]
series:
  - title: Traffic Source
    data: [55, 35, 10]
width: 50%
labelColors: true
```
````

### Doughnut Chart
Like pie but with a hollow center. Same syntax, just `type: doughnut`.

````yaml
```chart
type: doughnut
labels: [Rent, Food, Transport, Savings]
series:
  - title: Budget
    data: [40, 25, 15, 20]
width: 50%
```
````

### Radar Chart
Multi-axis comparison — each label is an axis radiating from center. Good for comparing profiles or skill sets.

````yaml
```chart
type: radar
labels: [Speed, Strength, Defense, Magic, Stamina]
series:
  - title: Warrior
    data: [6, 9, 8, 2, 7]
  - title: Mage
    data: [5, 3, 4, 10, 6]
width: 60%
```
````

### Polar Area Chart
Like radar but uses area/radius instead of a polygon. Each slice has equal angle but varying radius.

````yaml
```chart
type: polarArea
labels: [Red, Blue, Green, Yellow]
series:
  - title: Votes
    data: [12, 19, 8, 15]
width: 50%
```
````

### Sankey Chart
Flow diagrams showing quantities between nodes. Data format differs from other chart types — each data entry is a `[source, value, destination]` triple.

````yaml
```chart
type: sankey
labels: [Salary, Freelance, Rent, Food, Savings, Investment]
series:
  - data:
      - [Salary, 5000, Rent]
      - [Salary, 2000, Food]
      - [Salary, 1500, Savings]
      - [Freelance, 1000, Savings]
      - [Savings, 2000, Investment]
    colorFrom:
      Salary: "#4CAF50"
      Freelance: "#2196F3"
    colorTo:
      Rent: "#f44336"
      Food: "#FF9800"
      Savings: "#9C27B0"
      Investment: "#3F51B5"
```
````

## Modifiers Reference

### Dimensions & Layout
| Modifier | Values | Default | Applies To |
|----------|--------|---------|------------|
| `width` | CSS value (`400px`, `50%`) | `100%` | All (recommended for pie/doughnut/radar/polarArea) |
| `padding` | integer | — | All |
| `indexAxis` | `x`, `y` | `x` | Bar, Line (y = horizontal) |

### Legend
| Modifier | Values | Default | Applies To |
|----------|--------|---------|------------|
| `legend` | `true`, `false` | `true` | All |
| `legendPosition` | `top`, `bottom`, `left`, `right` | `top` | All |

### Axes (bar/line only)
| Modifier | Values | Default | Notes |
|----------|--------|---------|-------|
| `beginAtZero` | `true`, `false` | `false` | Force y-axis to start at 0 |
| `xTitle` / `yTitle` | string | — | Axis labels |
| `xMin` / `xMax` / `yMin` / `yMax` | number | — | Axis bounds |
| `xReverse` / `yReverse` | `true`, `false` | `false` | Reverse axis direction |
| `xDisplay` / `yDisplay` | `true`, `false` | `true` | Show/hide entire axis |
| `xTickDisplay` / `yTickDisplay` | `true`, `false` | `true` | Show/hide tick marks |
| `stacked` | `true`, `false` | `false` | Stack series |
| `time` | `day`, `week`, `month`, `year` | — | Auto-format date labels |

### Radar/Polar Area Axes
| Modifier | Values | Default |
|----------|--------|---------|
| `rMin` / `rMax` | number | — |

### Line Chart Specific
| Modifier | Values | Default | Notes |
|----------|--------|---------|-------|
| `fill` | `true`, `false` | `false` | Shade area under line |
| `tension` | 0–1 | 0 | Line smoothness |
| `spanGaps` | `true`, `false` | `false` | Connect across null data |
| `bestFit` | `true`, `false` | `false` | Add regression line |
| `bestFitTitle` | string | "Line of Best Fit" | Label for regression |
| `bestFitNumber` | integer | 0 | Which series to fit (0-indexed) |

### Appearance
| Modifier | Values | Default | Notes |
|----------|--------|---------|-------|
| `transparency` | 0.0–1.0 | 0.25 | Opacity of fill colors |
| `labelColors` | `true`, `false` | `false` | Color labels in pie/doughnut/polarArea |

## Chart from Table

Link a chart to a markdown table in the same note (or a different note) using block IDs. The chart reads data directly from the table — update the table, chart updates automatically.

### Step 1: Create the table with a block ID

```markdown
| Month | Revenue | Expenses |
| ----- | ------- | -------- |
| Jan   | 120     | 100      |
| Feb   | 150     | 110      |
| Mar   | 180     | 130      |
^financial-data
```

The `^financial-data` block ID goes on the line immediately after the table (no blank line between).

### Step 2: Reference the table in a chart

````yaml
```chart
type: bar
id: financial-data
layout: rows
beginAtZero: true
```
````

- `id` — matches the `^block-id` on the table
- `layout` — `rows` or `columns`, controls how table structure maps to chart data
  - `rows`: each row is a data point, first column = labels, remaining columns = series
  - `columns`: each column is a data point, first row = labels, remaining rows = series
- `select` — (optional) filter specific rows/columns: `select: [Jan, Feb]`

### Cross-note table reference

To reference a table in a different note, add `file`:

````yaml
```chart
type: line
id: brew-record
file: 2023-05-29 Klatch - ORG ESPRESSO
layout: columns
```
````

## Dataview Integration

For dynamic charts driven by vault data, use DataviewJS with the `window.renderChart()` API. Requires DataviewJS enabled in Dataview plugin settings.

### Basic: Chart from current page properties

````javascript
```dataviewjs
const data = dv.current()
const chartData = {
    type: 'bar',
    data: {
        labels: [data.test],
        datasets: [{
            label: 'Score',
            data: [data.mark],
            backgroundColor: ['rgba(75, 192, 192, 0.2)'],
            borderColor: ['rgba(75, 192, 192, 1)'],
            borderWidth: 1
        }]
    }
}
window.renderChart(chartData, this.container)
```
````

### Advanced: Aggregate across pages

Real vault data often has missing properties. Always filter nulls and add axis labels for clarity:

````javascript
```dataviewjs
const pages = dv.pages('#project').where(p => p.score)
const labels = pages.map(p => p.file.name).values
const scores = pages.map(p => p.score).values

const chartData = {
    type: 'bar',
    data: {
        labels: labels,
        datasets: [{
            label: 'Project Scores',
            data: scores,
            backgroundColor: 'rgba(54, 162, 235, 0.2)',
            borderColor: 'rgba(54, 162, 235, 1)',
            borderWidth: 1
        }]
    },
    options: {
        scales: {
            y: {
                beginAtZero: true,
                title: { display: true, text: 'Score' }
            },
            x: {
                title: { display: true, text: 'Project' }
            }
        },
        plugins: {
            title: { display: true, text: 'Project Scores Overview' }
        }
    }
}
window.renderChart(chartData, this.container)
```
````

Key patterns for the `renderChart` API:
- **Filter nulls:** `.where(p => p.property)` before mapping — pages missing the property cause chart errors
- **`options.scales`**: Configure axes (titles, min/max, beginAtZero) via Chart.js scale config
- **`options.plugins`**: Add chart title, customize legend, tooltip behavior
- Full Chart.js configuration works here — see [references/dataview-advanced.md](references/dataview-advanced.md) for dual axes, mixed chart types, and more

### Inline Dataview approach

Build YAML chart codeblocks dynamically — simpler syntax but fewer Chart.js options:

````javascript
```dataviewjs
const pages = dv.pages('#expense').where(p => p.amount)
const labels = pages.map(p => p.file.name).values
const amounts = pages.map(p => p.amount).values

dv.paragraph(`\`\`\`chart
type: pie
labels: [${labels.join(', ')}]
series:
  - title: Expenses
    data: [${amounts.join(', ')}]
width: 50%
\`\`\``)
```
````

## Theming & Colors

### Plugin Settings
Add/edit chart colors in the Charts plugin settings panel. Colors apply in order to series.

### CSS Variables
Enable "Theming" in plugin settings, then define custom colors in a CSS snippet:

```css
:root {
  --chart-color-1: #4CAF50;
  --chart-color-2: #2196F3;
  --chart-color-3: #f44336;
  --chart-color-4: #FF9800;
  --chart-color-5: #9C27B0;
}
```

### Image Export
Use the command palette: **"Create image from Chart"** to export any chart as PNG/JPG/WebP. Configure quality and format in plugin settings.

## Common Patterns

### Tracking data over time (table-linked)

Perfect for journals, habit tracking, coffee brewing logs, fitness records:

````markdown
# Progress Chart
```chart
type: line
id: tracking-data
layout: rows
tension: 0.3
beginAtZero: false
legend: true
```

# Records
| Date       | Weight | Steps |
| ---------- | ------ | ----- |
| 2024-01-01 | 82     | 5000  |
| 2024-01-08 | 81     | 6200  |
| 2024-01-15 | 80     | 7100  |
^tracking-data
````

### Budget/allocation (pie or doughnut)

````yaml
```chart
type: doughnut
labels: [Housing, Food, Transport, Entertainment, Savings]
series:
  - title: Monthly Budget
    data: [35, 25, 15, 10, 15]
width: 50%
labelColors: true
```
````

### Skill comparison (radar)

````yaml
```chart
type: radar
labels: [Communication, Technical, Leadership, Creativity, Teamwork]
series:
  - title: Self Assessment
    data: [7, 9, 6, 8, 7]
  - title: Peer Assessment
    data: [8, 8, 7, 7, 8]
width: 60%
```
````

### Side-by-side comparison (stacked bar)

````yaml
```chart
type: bar
labels: [Feature A, Feature B, Feature C, Feature D]
series:
  - title: Product X
    data: [85, 70, 90, 60]
  - title: Product Y
    data: [75, 85, 70, 80]
stacked: false
beginAtZero: true
```
````

## Tips & Gotchas

- **Pie/doughnut/radar/polarArea sizing:** Always set `width` (e.g., `50%`, `400px`). Without it, these charts stretch to full container width and look oversized.
- **Prefer inline YAML arrays:** Use `labels: [A, B, C]` and `data: [1, 2, 3]` instead of multi-line YAML lists. It's more compact and easier to read in chart codeblocks.
- **YAML indentation matters:** Series items must be indented consistently. Use 2-space indentation.
- **Block ID placement:** For table-linked charts, the `^block-id` must be on the line immediately after the table with no blank line between.
- **Copy-paste caution:** Pasting chart YAML from external sources can introduce invisible characters or wrong indentation. If a chart doesn't render, re-type the codeblock manually.
- **Sankey data format** is different from all other charts — `data` entries are `[source, value, destination]` triples, not simple numbers.
- **Multiple series** share the same `labels` array. Every series `data` array must have the same length as `labels`.
- **DataviewJS requirement:** The `window.renderChart()` API requires DataviewJS to be enabled in Dataview settings (not just Dataview itself).
- **Filter nulls in Dataview:** Always use `.where(p => p.property)` before `.map()` — pages missing the property produce `undefined` values that break charts.

## References

- [Charts Plugin Documentation](https://charts.phib.ro/Meta/Charts/Charts+Documentation)
- [Chart.js Documentation](https://www.chartjs.org/docs/latest/)
- [references/dataview-advanced.md](references/dataview-advanced.md) — Advanced Dataview integration patterns
