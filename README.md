# chart.registry.js

This is a proposal to move the chart registry out of dc.js into its own module, to create
a standard for linked charts that's not bound to any one library.

I haven't started with making this a npm package/module, but the intention is for this to
depend on [d3-dispatch](https://github.com/d3/d3-dispatch) only, and even that is an implicit dependency.

If this succeeds it will be moved to the [dc-js organization](https://github.com/dc-js).

## Usage

The chart registry contains "types" of "groups". A group is an instance of a type and usually
an instance of `d3.dispatch`. The type specifies how to construct the group. For example, the
underlying implementation for `dc.chartRegistry` can be instantiated like so:

```js
var chartRegistry_store = chart_registry.create_type('dc-chart-registry', function() {
    return d3.dispatch('filterAll', 'focus', 'render', 'redraw');
});
```

Once a type is instantiated, groups can be added using `create_group`:

```js
var chartgroup = chart_registry.create_group('dc-chart-registry', 'chartgroup1');
```

Registering the individual charts is now just subscribing to events:

```js
chartgroup.on('filterAll', chart.filterAll.bind(chart))
```
