# Part 2: Bar charts, data shapes

```js
const summary = FileAttachment("data/google-analytics-summary.csv").csv({typed: true});
const hourly = FileAttachment("data/google-analytics-time-of-day.csv").csv({typed: true});
const channelBreakdown = FileAttachment("data/google-analytics-channel-breakdown.csv").csv({typed: true});
const cbUnpivoted = FileAttachment("data/channel-breakdown-unpivoted.csv").csv({typed: true});
```

```js
function simpleBars(width) {
    return Plot.plot({
        width,
        marks: [
                Plot.barY(channelBreakdown, Plot.groupX({y: "sum"}, {x: "channelGroup", y: "active28d", tip: true})),
                Plot.ruleY([0])
        ],
        y: { tickFormat: "s" },
        color: { legend: true }
    });
}

function lollipop(width) {
    return Plot.plot({
        width,
        marks: [
                Plot.ruleX(channelBreakdown, Plot.groupX({y: "sum"}, {x: "channelGroup", strokeWidth: 10, stroke: "steelblue", y: "active28d", tip: true})),
                Plot.dot(channelBreakdown, Plot.groupX({y: "sum"}, {x: "channelGroup", r: 20, fill: "steelblue", y: "active28d", tip: true})),
                Plot.ruleY([0])
        ],
        y: { tickFormat: "s" },
        color: { legend: true }
    });
}

function horizontalBars(width) {
    return Plot.plot({
        marks: [
            Plot.barX(channelBreakdown, {y: "channelGroup", x: "active28d", inset: 0.5, fill: "type", sort: "active28d"}),
            Plot.ruleX([0])
        ],
        marginLeft: 100
    });
}

function stackedBars(width) {
    return Plot.plot({
        width,
        marks: [
                Plot.barY(channelBreakdown, {x: "channelGroup", y: "active28d", inset: 0.5, fill: "type", tip: true}),
                Plot.ruleY([0])
        ],
        y: { tickFormat: "s" },
        color: { legend: true }
    });
}

function facetedBars(width) {
    return Plot.plot({
        width,
        marks: [
            Plot.barY(channelBreakdown, {x: "channelGroup", y: "active28d", inset: 0.5, fy: "type", fill: "type", tip: true}),
            Plot.ruleY([0])
    ],
    y: { tickFormat: "s" },
    marginRight: 70
    });
}
```

<div class="grid" style="grid-auto-rows: auto;">
    <div class="card">
        <h2>Bar (column) chart</h2>
        ${resize(simpleBars)}
    </div>
    <div class="card">
        <h2>Lollipop chart</h2>
        ${resize(lollipop)}
    </div>
    <div class="card">
        <h2>Stacked bar chart</h2>
        ${resize(horizontalBars)}
    </div>
    <div class="card">
        <h2>Stacked bar chart</h2>
        ${resize(stackedBars)}
    </div>
    <div class="card">
        <h2>Faceted bar chart</h2>
        ${resize(facetedBars)}
    </div>
    <div class="card">
        <h2>Data table, wide format</h2>
        ${Inputs.table(cbUnpivoted)}
    </div>
    <div class="card">
        <h2>Data table, long format</h2>
        ${Inputs.table(channelBreakdown)}
    </div>
</div>