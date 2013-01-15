---
layout: article
title: CS 140 Homework 1
---

All data is available in Google Docs at <http://j.mp/matrixdata>.

A version of this document with interactive graphs is available at <http://andresriofrio.com/cs140-hw1/>.

## Modifications to the code

 1. Extend option parsing and testing code to run a configurable number of
    iterations.
 2. Print the time elapsed in microseconds instead of seconds.
 3. Temporarily modified `matrix_multiply_run_1()` to test different nesting permutations.

## Part 1: Increasing matrix size

We can see here that the algorithm is not exactly *O(n<sup>3</sup>)* as advertised. It's a bit faster for matrices of sizes between 8 and 512 (as pictured). Alternatively, it's slower for 2x2 matrices and matrices larger than 512x512.

<script type="text/javascript" src="//ajax.googleapis.com/ajax/static/modules/gviz/1.0/chart.js">
{
  "dataSourceUrl": "//docs.google.com/spreadsheet/tq?range=A1%3AC12&key=0Aktsct0Ua9XhdHRHcXZrRldURm91azhnbUNVM2NTanc&gid=0&headers=-1",
  "chartType": "LineChart",
  "options": {
    "titleTextStyle": {"fontSize": 16},
    "vAxis": {"title": "Execution time (μs)", "logScale": true, "format": "#,###"},
    "hAxis": {"title": "Matrix size", "logScale": true},
    "title": "Log-log plot of execution time and matrix size",
    "width": 700, "height": 400,
    "pointSize": 7,
    "series": [
      {}, {"pointSize": 0, "lineWidth": 1}
    ]
  }
}
</script>

The performance vs. matrix size plot is not a horizontal line, as expected, showing again that the algorithm is not exactly *O(n<sup>3</sup>)*. I can hypothesise that, for smaller matrices, memory can be cached very well, and that's why larger matrices have worse performance. However, I'm not sure why very small matrices have worse performance. Perhaps it's a problem with my benchmarking code, but I haven't looked into it.

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
    "width": 700, "height": 400,
    "legend": "none",
    "pointSize": 7,
    "series": [
      {"lineWidth": 1}
    ]
  }
}
</script>

## Part 2: Nesting permutatations

In the chart above we see that the complexity of the algorithm remains very close to *O(n<sup>3</sup>)* for any nesting permutation. Still, the permutations `kij` and `jik` show a larger average logarithmic slope than the rest, becoming specially apparent in the larger matrix sizes. For a 2048x2048 matrix, the `jki` permutation is 20 times faster than the `kij` permutation.

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
    "width": 700, "height": 400,
    "lineWidth": 1,
    "pointSize": 7
  }
}
</script>

This chart lets us see the difference between nesting permutations more clearly. We can see that, while `jki` is fastest for large matrices, its a bit slower for small matrices.

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
    "width": 700, "height": 400,
    "lineWidth": 1,
    "pointSize": 7
  }
}
</script>

### Why is `jki` faster?

Since the matrices are represented in memory one row after another, elements in the same row are close to each other in memory, which means that once one element in a row is accessed, other elements in the same row are likely to have been pulled into the cache along with them. The code that does the actual multiplication inside the for loops is:

    element(C,i,j) += element(A,i,k) * element(B,k,j);

We want to maximize the closeness of subsequent lookups, so we want to either put `i` or `k` in the innermost loop. Since `i` is used to index both `A` and `C`, we choose that. Then we can choose `k`. Finally, `j` can be in the outermost loop.

Of course, this is an ad-hoc explanation, and there could be other ways of generating the output matrix that take even better advantage of memory caching by having greater locality of reference.

