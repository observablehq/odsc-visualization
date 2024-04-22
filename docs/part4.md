# Part 4: Encodings, interaction

```js
const summary = FileAttachment("data/google-analytics-summary.csv").csv({typed: true});
const hourly = FileAttachment("data/google-analytics-time-of-day.csv").csv({typed: true});
const channelBreakdown = FileAttachment("data/google-analytics-channel-breakdown.csv").csv({typed: true});
const anscombe = FileAttachment("data/anscombe.csv").csv({typed: true});
```

```js
function logLineChart(width) {
    return Plot.plot({
        width,
        y: {type: "log", grid: true},
        marks: [
            Plot.lineY(summary, {x: "date", y: "active28d"})
        ]
    });
}

function logLineChartToolTip(width) {
    return Plot.plot({
        width,
        y: {type: "log", grid: true},
        marks: [
            Plot.lineY(summary, {x: "date", y: "active28d", tip: true,
            title: d => d.date.toLocaleDateString()+": "+d.active28d})
        ]
    });
}
```

<div class="grid" style="grid-auto-rows: auto;">
    <div class="card">
        <h2>Line chart</h2>
        ${resize(w => logLineChart(w))}
    </div>
    <div class="card">
        <h2>Line chart with tooltip</h2>
        ${resize(w => logLineChartToolTip(w))}
    </div>
</div>