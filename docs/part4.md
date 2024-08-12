# Part 4: Encodings, interaction

```js
const summary = FileAttachment("data/google-analytics-summary.csv").csv({typed: true});
const hourly = FileAttachment("data/google-analytics-time-of-day.csv").csv({typed: true});
const channelBreakdown = FileAttachment("data/google-analytics-channel-breakdown.csv").csv({typed: true});
const anscombe = FileAttachment("data/anscombe.csv").csv({typed: true});
```

## Static line chart (using log scale)
```js echo
Plot.plot({
    width,
    y: {type: "log", grid: true},
    marks: [
        Plot.lineY(summary, {x: "date", y: "active28d"})
    ]
})
```

## Interactive line chart with tooltip
```js echo
Plot.plot({
    width,
    y: {type: "log", grid: true},
    marks: [
        Plot.lineY(summary, {x: "date", y: "active28d", tip: true,
        title: d => d.date.toLocaleDateString()+": "+d.active28d})
    ]
})
```
