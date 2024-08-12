# Part 1: Basics, scatterplot

```js
const summary = FileAttachment("data/google-analytics-summary.csv").csv({typed: true});
const hourly = FileAttachment("data/google-analytics-time-of-day.csv").csv({typed: true});
const channelBreakdown = FileAttachment("data/google-analytics-channel-breakdown.csv").csv({typed: true});
const anscombe = FileAttachment("data/anscombe.csv").csv({typed: true});
```

## Anscombe's quartet

The data:
```js
Inputs.table(anscombe)
```

Or, in charts:
```js echo
Plot.plot({
    grid: true,
    inset: 10,
    width: 928,
    height: 240,
    facet: {
        data: anscombe,
        x: "group"
    },
    marks: [
        Plot.frame(),
        Plot.dot(anscombe, {x: "x", y: "y"}),
        Plot.linearRegressionY(anscombe, {x: "x", y: "y", stroke: "steelblue", ci: 0})
    ],
})
```

## Scatterplot

```js echo
Plot.plot({
    width,
    marks: [
        Plot.dot(hourly, {x: "activeUsers", y: "newUsers"}),
        Plot.linearRegressionY(hourly, {x: "activeUsers", y: "newUsers", stroke: "steelblue", ci: 0})
    ]
})
```
