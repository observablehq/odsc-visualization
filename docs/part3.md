# Part 3: Line charts, time

```js
const summary = FileAttachment("data/google-analytics-summary.csv").csv({typed: true});
const hourly = FileAttachment("data/google-analytics-time-of-day.csv").csv({typed: true});
const channelBreakdown = FileAttachment("data/google-analytics-channel-breakdown.csv").csv({typed: true});
const anscombe = FileAttachment("data/anscombe.csv").csv({typed: true});
```

```js

function lineChart(width, height) {
    return Plot.plot({
        width,
        height,
        marks: [
            Plot.ruleY([0]),
            Plot.lineY(summary, {x: "date", y: "active28d"})
        ]
    });
}

function areaChart(width, height) {
    return Plot.plot({
        width,
        height,
        marks: [
            Plot.areaY(summary, {x: "date", y: "active28d", fill: "steelblue"}),
        ]
    });
}

function monthlyLineChart(width) {
    return Plot.plot({
        width,
        y: { tickFormat: "s"},
        marks: [
            Plot.ruleY([0]),
            Plot.lineY(summary, Plot.binX({y: "sum"}, {x: "date", interval: "month", y: "active28d"}))
        ]
    });
}

function monthlyBarChart(width) {
    return Plot.plot({
        width,
        y: { tickFormat: "s"},
        marks: [
            Plot.rectY(summary, Plot.binX({y: "sum"}, {x: "date", interval: "month", y: "active28d"})),
        ]
    });
}

function facetedLines(width) {
    return Plot.plot({
        width,
        marginRight: 60,
        fy: {label: null, domain: ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]},
        marks: [
            Plot.areaY(hourly, {x: "hour", y: "activeUsers", fy: "dayOfWeek", curve: "basis", sort: "hour", fill: "dayOfWeek"}),
        ]
    });
}

function multiLines(width) {
    return Plot.plot({
        width,
        color: {domain: ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"], legend: true},
        marks: [
            Plot.ruleY([0]),
            Plot.lineY(hourly, {x: "hour", y: "activeUsers", sort: "hour", strokeWidth: 1, stroke: "dayOfWeek"})
        ]
    });
}


function ridgeLine(width) {
    return Plot.plot({
        height: 20 + new Set(hourly.map(d => d.dayOfWeek)).size * 40,
        width,
        marginBottom: 1,
        marginLeft: 80,
        x: {axis: "top"},
        y: {axis: null, range: [2.5 * 25 - 2, (2.5 - overlap ) * 25 - 2]},
        fy: {label: null, domain: ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]},
        marks: [
            Plot.areaY(hourly, {x: "hour", y: "activeUsers", fy: "dayOfWeek", curve: "basis", sort: "hour", fill: "dayOfWeek"}),
            // Plot.ruleY([0], {stroke: "lightgray"}),
            Plot.lineY(hourly, {x: "hour", y: "activeUsers", fy: "dayOfWeek", curve: "basis", sort: "hour", strokeWidth: 1, stroke: "white"})
        ]
    });
}

function connectedScatter(width) {
    return Plot.plot({
        inset: 10,
        grid: true,
        marks: [
            Plot.line(summary, {x: "active28d", y: "wauPerMau", curve: "catmull-rom", marker: true}),
            // Plot.text(summary, {x: "active28d", y: "wauPerMau", text: (d) => `${d.date.getMonth()}`, dy: -6, lineAnchor: "bottom"})
        ]
    })
}

```


<div class="grid" style="grid-auto-rows: auto;">
    <div class="card">
        <h2>Line chart</h2>
        ${resize(w => lineChart(w))}
    </div>
    <div class="card">
        <h2>Line chart, compressed</h2>
        ${resize(w => lineChart(w/2))}
    </div>
    <div class="card">
        <h2>Line chart, flat</h2>
        ${resize(w => lineChart(w, 100))}
    </div>
    <div class="card">
        <h2>Area chart</h2>
        ${resize(w => areaChart(w))}
    </div>
    <div class="card">
        <h2>Monthly line chart</h2>
        ${resize(w => monthlyLineChart(w))}
    </div>
    <div class="card">
        <h2>Monthly bar chart</h2>
        ${resize(w => monthlyBarChart(w))}
    </div>
    <div class="card">
        <h2>Multiple line chart</h2>
        ${resize(w => multiLines(w))}
    </div>
    <div class="card">
        <h2>Faceted line chart</h2>
        ${resize(w => facetedLines(w))}
    </div>
</div>

```js
const overlap = view(Inputs.range([1, 3], {label: "Overlap:", step: .1, value: 2}));
```
<div class="grid" style="grid-auto-rows: auto;">
    <div class="card">
        <h2>Ridgeline plot (active users per hour)</h2>
        ${resize(ridgeLine)}
    </div>
    <div class="card">
        <h2>Connected scatterplot</h2>
        ${resize(connectedScatter)}
    </div>
</div>

