---
layout: article
title: CS 140 Homework 1
---

All data is available in Google Docs at <http://j.mp/matrixdata>.

## Modifications to the code

 1. Extend option parsing and testing code to run a configurable number of
    iterations.
 2. Print the time elapsed in microseconds instead of seconds.
 3. Temporarily modified `matrix_multiply_run_1()` to test different nesting permutations.

## Part 1: Increasing matrix size

<script type="text/javascript" src="//ajax.googleapis.com/ajax/static/modules/gviz/1.0/chart.js">
{
  "dataSourceUrl": "//docs.google.com/spreadsheet/tq?range=A1%3AC12&key=0Aktsct0Ua9XhdHRHcXZrRldURm91azhnbUNVM2NTanc&gid=0&headers=-1",
  "chartType": "LineChart",
  "options": {
    "titleTextStyle": {"fontSize": 16},
    "vAxis": {"title": "Execution time (μs)", "logScale": true, "format": "#,###"},
    "hAxis": {"title": "Matrix size", "logScale": true},
    "title": "Log-log plot of execution time and matrix size",
    "width": 900, "height": 400,
    "pointSize": 7,
    "series": [
      {}, {"pointSize": 0, "lineWidth": 1}
    ]
  }
}
</script>

<script type="text/javascript" src="//ajax.googleapis.com/ajax/static/modules/gviz/1.0/chart.js">
{
  "dataSourceUrl": "//docs.google.com/spreadsheet/tq?range=H1%3AI12&key=0Aktsct0Ua9XhdHRHcXZrRldURm91azhnbUNVM2NTanc&gid=0&headers=-1",
  "chartType": "LineChart",
  "options": {
    "titleTextStyle": {"fontSize": 16},
    "curveType": "function",
    "vAxis": {"title": "Performance (flops)", "format": "#,###"},
    "hAxis": {"title": "Matrix size", "logScale": true},
    "title": "Log-linear plot of performance and matrix size",
    "width": 900, "height": 400,
    "legend": "none",
    "pointSize": 7,
    "series": [
      {"lineWidth": 1}
    ]
  }
}
</script>

The performance vs. matrix size plot is not a horizontal line, as expected, because <mark>...</mark>

## Part 2: Nesting permutatations

<script type="text/javascript" src="//ajax.googleapis.com/ajax/static/modules/gviz/1.0/chart.js">
{
  "dataSourceUrl": "//docs.google.com/spreadsheet/tq?range='Part+2'!A1%3AG12&key=0Aktsct0Ua9XhdHRHcXZrRldURm91azhnbUNVM2NTanc&gid=0&headers=-1",
  "chartType": "LineChart",
  "options": {
    "titleTextStyle": {"fontSize": 16},
    "curveType": "function",
    "vAxis": {"title": "Execution time (μs)", "logScale": true, "format": "#,###"},
    "hAxis": {"title": "Matrix size", "logScale": true},
    "title": "Log-log plot of execution time and matrix size for different nesting permutations",
    "width": 900, "height": 400,
    "lineWidth": 1,
    "pointSize": 7
  }
}
</script>

In the chart above we see that the complexity of the algorithm remains very
close to *O(n<sup>3</sup>)* for any nesting permutation. Still, the
permutations `kij` and `jik` show a larger average logarithmic slope than the
rest, becoming specially apparent in the larger matrix sizes. For a 2048x2048
matrix, the `jki` permutation is 20 times faster than the `kij` permutation.

<script type="text/javascript" src="//ajax.googleapis.com/ajax/static/modules/gviz/1.0/chart.js">
{
  "dataSourceUrl": "//docs.google.com/spreadsheet/tq?range='Part+2'!A1%3AG12&key=0Aktsct0Ua9XhdHRHcXZrRldURm91azhnbUNVM2NTanc&gid=0&headers=1&transpose=1",
  "chartType": "LineChart",
  "options": {
    "titleTextStyle": {"fontSize": 16},
    "curveType": "function",
    "vAxis": {"title": "Execution time (μs)", "logScale": true, "format": "#,###"},
    "hAxis": {"title": "Nesting permutation"},
    "title": "Log chart of execution time vs. nesting permutations",
    "width": 900, "height": 400,
    "lineWidth": 1,
    "pointSize": 7
  }
}
</script>

<script type="text/javascript" src="//ajax.googleapis.com/ajax/static/modules/gviz/1.0/chart.js">
{
  "dataSourceUrl": "//docs.google.com/spreadsheet/tq?range=A1%3AG1%2CA6%3AG6%2CA9%3AG9%2CA12%3AG12&merge=ROWS&key=0Aktsct0Ua9XhdHRHcXZrRldURm91azhnbUNVM2NTanc&gid=1&headers=1&transpose=1",
  "chartType": "ColumnChart",
  "options": {
    "titleTextStyle": {"fontSize": 16},
    "curveType": "function",
    "vAxis": {"title": "Execution time (μs)", "logScale": true, "format": "#,###"},
    "hAxis": {"title": "Nesting permutation"},
    "title": "Log chart of execution time vs. nesting permutations",
    "width": 900, "height": 400,
    "lineWidth": 1
  }
}
</script>

## Optimization (Extra credit)

