# Advanced Dataview Integration for Charts

This reference covers complex patterns for using the Charts plugin with DataviewJS.

## Table of Contents
- [Multi-page aggregation](#multi-page-aggregation)
- [Grouped/filtered data](#groupedfiltered-data)
- [Dynamic chart type selection](#dynamic-chart-type-selection)
- [Full Chart.js config](#full-chartjs-config)

## Multi-page Aggregation

Collect data across many notes and chart it:

````javascript
```dataviewjs
const pages = dv.pages('#daily-log')
const dates = pages.sort(p => p.file.name).map(p => p.file.name).values
const mood = pages.sort(p => p.file.name).map(p => p.mood).values
const energy = pages.sort(p => p.file.name).map(p => p.energy).values

const chartData = {
    type: 'line',
    data: {
        labels: dates,
        datasets: [
            {
                label: 'Mood',
                data: mood,
                borderColor: 'rgba(75, 192, 192, 1)',
                backgroundColor: 'rgba(75, 192, 192, 0.2)',
                tension: 0.3,
                fill: true
            },
            {
                label: 'Energy',
                data: energy,
                borderColor: 'rgba(255, 159, 64, 1)',
                backgroundColor: 'rgba(255, 159, 64, 0.2)',
                tension: 0.3,
                fill: true
            }
        ]
    },
    options: {
        scales: {
            y: { beginAtZero: true, max: 10 }
        }
    }
}
window.renderChart(chartData, this.container)
```
````

## Grouped/Filtered Data

Filter pages by tag or property, group, and aggregate:

````javascript
```dataviewjs
// Count notes per tag category
const pages = dv.pages('#project')
const statusCounts = {}
for (const p of pages) {
    const status = p.status || 'unknown'
    statusCounts[status] = (statusCounts[status] || 0) + 1
}

const chartData = {
    type: 'doughnut',
    data: {
        labels: Object.keys(statusCounts),
        datasets: [{
            data: Object.values(statusCounts),
            backgroundColor: [
                'rgba(76, 175, 80, 0.7)',
                'rgba(33, 150, 243, 0.7)',
                'rgba(255, 152, 0, 0.7)',
                'rgba(244, 67, 54, 0.7)'
            ]
        }]
    }
}
window.renderChart(chartData, this.container)
```
````

## Dynamic Chart Type Selection

Use a frontmatter property to control chart type:

````javascript
```dataviewjs
const chartType = dv.current().chartType || 'bar'
const labels = dv.current().chartLabels || ['A', 'B', 'C']
const data = dv.current().chartData || [1, 2, 3]

const chartData = {
    type: chartType,
    data: {
        labels: labels,
        datasets: [{
            label: 'Data',
            data: data,
            backgroundColor: 'rgba(54, 162, 235, 0.5)',
            borderColor: 'rgba(54, 162, 235, 1)',
            borderWidth: 1
        }]
    }
}
window.renderChart(chartData, this.container)
```
````

## Full Chart.js Config

The `renderChart` API accepts any valid Chart.js configuration. This means you can use:

- Custom tooltips
- Animation settings
- Multiple y-axes
- Plugin configurations
- Click handlers (limited in Obsidian context)

Example with dual axes:

````javascript
```dataviewjs
const chartData = {
    type: 'bar',
    data: {
        labels: ['Jan', 'Feb', 'Mar', 'Apr'],
        datasets: [
            {
                label: 'Revenue ($)',
                data: [12000, 15000, 18000, 22000],
                backgroundColor: 'rgba(54, 162, 235, 0.5)',
                yAxisID: 'y'
            },
            {
                label: 'Growth (%)',
                data: [5, 8, 12, 15],
                type: 'line',
                borderColor: 'rgba(255, 99, 132, 1)',
                yAxisID: 'y1'
            }
        ]
    },
    options: {
        scales: {
            y: {
                type: 'linear',
                position: 'left',
                title: { display: true, text: 'Revenue ($)' }
            },
            y1: {
                type: 'linear',
                position: 'right',
                title: { display: true, text: 'Growth (%)' },
                grid: { drawOnChartArea: false }
            }
        }
    }
}
window.renderChart(chartData, this.container)
```
````
