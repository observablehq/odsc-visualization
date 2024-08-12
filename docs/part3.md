# Part 3: Line charts, time

```js
const summary = FileAttachment("data/google-analytics-summary.csv").csv({typed: true});
const hourly = FileAttachment("data/google-analytics-time-of-day.csv").csv({typed: true});
const channelBreakdown = FileAttachment("data/google-analytics-channel-breakdown.csv").csv({typed: true});
const anscombe = FileAttachment("data/anscombe.csv").csv({typed: true});
```

### Line chart and banking to 45º

Line chart function
```js echo
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
```

Close to 45º on average:
```js echo
lineChart(width)
```

Compressed horizontally:
```js echo
lineChart(width/2)
```

Conpressed vertically:
```js echo
lineChart(width, 100)
```

## Area chart

```js echo
Plot.plot({
    width,
    marks: [
        Plot.areaY(summary, {x: "date", y: "active28d", fill: "steelblue"}),
    ]
})
```

## Monthly line vs. bar chart

```js echo
Plot.plot({
    width,
    y: { tickFormat: "s"},
    marks: [
        Plot.ruleY([0]),
        Plot.lineY(summary, Plot.binX({y: "sum"}, {x: "date", interval: "month", y: "active28d"}))
    ]
})
```

```js echo
Plot.plot({
    width,
    y: { tickFormat: "s"},
    marks: [
        Plot.rectY(summary, Plot.binX({y: "sum"}, {x: "date", interval: "month", y: "active28d"})),
    ]
})
```

## Multiple line chart

```js echo
Plot.plot({
        width,
        color: {domain: ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"], legend: true},
        marks: [
            Plot.ruleY([0]),
            Plot.lineY(hourly, {x: "hour", y: "activeUsers", sort: "hour", strokeWidth: 1, stroke: "dayOfWeek"})
        ]
    })
```

## Faceted line chart

```js echo
Plot.plot({
    width,
    marginRight: 60,
    fy: {label: null, domain: ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]},
    marks: [
        Plot.areaY(hourly, {x: "hour", y: "activeUsers", fy: "dayOfWeek", curve: "basis", sort: "hour", fill: "dayOfWeek"}),
    ]
})
```

## Ridgeline plot

```js
const overlap = view(Inputs.range([1, 3], {label: "Overlap:", step: .1, value: 2}));
```

```js echo
Plot.plot({
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
})
```

## Connected scatterplot

```js echo
Plot.plot({
    inset: 10,
    grid: true,
    marks: [
        Plot.line(summary, {x: "active28d", y: "wauPerMau", curve: "catmull-rom", marker: true}),
        // Plot.text(summary, {x: "active28d", y: "wauPerMau", text: (d) => `${d.date.getMonth()}`, dy: -6, lineAnchor: "bottom"})
    ]
})
```
