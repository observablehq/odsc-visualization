# Part 1: Basics, scatterplot

```js
const summary = FileAttachment("data/google-analytics-summary.csv").csv({typed: true});
const hourly = FileAttachment("data/google-analytics-time-of-day.csv").csv({typed: true});
const channelBreakdown = FileAttachment("data/google-analytics-channel-breakdown.csv").csv({typed: true});
const anscombe = FileAttachment("data/anscombe.csv").csv({typed: true});
```

```js
function anscombePlots(width) {
    return Plot.plot({
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
    });
}

function scatterPlot(width) {
    return Plot.plot({
        width,
        marks: [
            Plot.dot(hourly, {x: "activeUsers", y: "newUsers"}),
            Plot.linearRegressionY(hourly, {x: "activeUsers", y: "newUsers", stroke: "steelblue", ci: 0})
        ]
    });
}
```

<div class="grid" style="grid-auto-rows: auto;">
  <div class="card">
    <h2>Anscombe's quartet, data</h2>
    ${Inputs.table(anscombe)}
  </div>
  <div class="card">
    <h2>Anscombe's quartet, charts</h2>
    ${resize(anscombePlots)}
  </div>
  <div class="card">
    <h2>Scatterplot</h2>
    ${resize(scatterPlot)}
  </div>
</div>