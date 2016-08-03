# React + Redux


## Redux
##### redux-trunk
[Read this for an in-depth introduction to thunks in Redux.](http://stackoverflow.com/questions/35411423/how-to-dispatch-a-redux-action-with-a-timeout/35415559#35415559)

Only plain object will go to store. A function will be called by redux-trunk and has no relationship with store itself except the dispatch argument.
Maybe we could consider it as a dispatch chain.



##### Work with old school library
When I try to integrate highcharts 3 into react-redux framework, I meet the issue that it cannot be import as a component (It could be by imports-loader, but I haven't try it. Then I load the highcharts in the old way:
```html
<script type="text/javascript" src="vendor/Highcharts-3.0.9/js/adapters/standalone-framework.src.js"></script>
<script type="text/javascript" src="vendor/Highcharts-3.0.9/js/highcharts.src.js"></script>
```

Then I have some code to get it work, for initial rendering.

In the owner container, I have
```javascript
getCharts() {
  return Object.keys(this.props.testChartData).sort().map((version) => {
    var args = {
      version,
      data: this.props.testChartData[version]
    };

    return <Chart key={version + Date.now()} {...args} />
  });
}
```

In the chart component, I have
```javascript
componentDidMount() {
  this.chart = new Highcharts.Chart(this.getChartOption());
}

render() {
  return (
    <div id={this.chartId()} className="col-md-12">chartId</div>
  );
}
```
Okay, this seems a combine of react and standard DOM operation. Then I meet a question - **How to update the charts with new  data?**

When new data set arrives, the behavior are interesting - 1) state doesn't change: nothing will be rendered, which make sense. 2) state changed: `render()` is called and new components are generated. **But the charts in browser is not updated!!!**

After I changed code to `return <Chart key={version + Date.now()} {...args} />`, I can see the rendering. I guess react doesn't consider it a necessary to update the DOM if we have same key assigned.

Right now, even though the charts are updated as expected, two things are remained to resolve:
* use version + Date.now() is a work around and is ugly and no meaningful, should update this.
* Render new charts every time is really expensive, I should use API of highcharts to update them instead of rerendering everything.
