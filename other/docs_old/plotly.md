# Visualizing Financial Data and Technical Indicators

## Using Plotly Library 
To effectively present the retrieved financial data and calculated technical indicators (such as RSI and SMA), I have utilized the Plotly library. 

Plotly is a powerful JavaScript library for creating interactive and dynamic visualizations that can be easily embedded in web application. It provides support for a wide range of charts and graphs, such as line charts, candlestick charts, heat maps, and area maps.

In order to integrate Plotly in the application : 
```
<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
```

First, common parameters such as config, layout are define as follow: 

```javascript
// CONFIG
const config = {
  displayModeBar: true, // Show the toolbar
  modeBarButtonsToRemove: [
    "toggleSpikelines", // Remove toggle spike lines
    "hoverClosestCartesian", // Remove closest hover
    "hoverCompareCartesian", // Remove compare hover
    "select2d", // Remove select box tool
    "lasso2d", // Remove lasso select tool
    "autoScale2d", // Remove auto scale tool
    "zoom2d", // Remove zoom box tool
    "hoverClosestGl2d", // Remove hover closest for GL (not relevant in 2D)
    "hoverClosestPie", // Remove hover closest for pie charts
    "toggleHover", // Remove hover toggle button
    "resetViews", // Remove reset views (multi-chart layouts)
    "sendDataToCloud", // Remove send data to cloud
    "editInChartStudio", // Remove Chart Studio editor
    "hoverClosest3d", // Remove 3D hover closest button
    "hoverClosestGeo", // Remove geo hover closest
    "hoverClosestGl2d", // Remove GL 2D hover closest
    "hoverClosestMapbox", // Remove mapbox hover closest
    "zoomInGeo", // Remove geo zoom in (for map charts)
    "zoomOutGeo", // Remove geo zoom out
    "resetGeo", // Remove geo reset
  ],
  displaylogo: false, // Remove "Produced with Plotly" logo
  responsive: true, // Make the chart responsive
  autosize: true, 
};
```

```javascript
//BASELAYOUT
const baseLayout = {
  yaxis: {
    title: "Stock Price (USD)",
    gridcolor: isDarkMode ? "#444" : "#e5e5e5",
  },
  xaxis: { gridcolor: isDarkMode ? "#444" : "#e5e5e5" },
  showlegend: true,
  legend: {
    orientation: "h",
    x: 0.5,
    xanchor: "center",
    y: 1.15,
    yanchor: "top",
    font: { size: 12, color: isDarkMode ? "white" : "black" },
  },
  updatemenus: [
    {
      type: "buttons",
      buttons: [
        {
          label: "1D",
          method: "update",
          args: [{ visible: [true, false, false, false, false] }],
        },
        {
          label: "1M",
          method: "update",
          args: [{ visible: [false, true, false, false, false] }],
        },
        {
          label: "3M",
          method: "update",
          args: [{ visible: [false, false, true, false, false] }],
        },
        {
          label: "1Y",
          method: "update",
          args: [{ visible: [false, false, false, true, false] }],
        },
        {
          label: "5Y",
          method: "update",
          args: [{ visible: [false, false, false, false, true] }],
        },
      ],
      direction: "left",
      x: 0.5,
      xanchor: "center",
      y: -0.15,
      yanchor: "top",
      pad: { r: 10, t: 10 },
      showactive: true,
      bgcolor: isDarkMode ? "black" : "white", // Black background for dark mode
      bordercolor: "#f4da40",
      borderwidth: 1,
      font: { size: 12, color: isDarkMode ? "yellow" : "black" }, // Yellow text for dark mode
      active: {
        bgcolor: isDarkMode ? "#111111" : "#ddd", // Slightly lighter black for active button
        font: { color: isDarkMode ? "yellow" : "#000000" }, // Yellow text for active button in dark mode
      },
    },
  ],
};
```

Furthermore, specific style layout objects are defined to obtain light and dark mode styles. 

```javascript
const lightModeStyles = {
  paper_bgcolor: "white",
  plot_bgcolor: "white",
  font: { color: "black" },
  button: {
    bgcolor: "#f2f2f2",
    textcolor: "#000000",
    hoverbgcolor: "#e0e0e0",
    activebgcolor: "#ddd",
  },
};

const darkModeStyles = {
  paper_bgcolor: "#1e1e1e",
  plot_bgcolor: "#1e1e1e",
  font: { color: "white" },
  button: {
    bgcolor: "#000000", // Pure black button background
    textcolor: "#FFFFFF", // White text
    hoverbgcolor: "#333333", // Dark gray for hover
    activebgcolor: "#111111", // Slightly lighter black for active
  },
};
```

At last, all the parameter objects are utlized in order to dynamically rendered the charts similar to this : 
```javascript
const traces = Object.keys(chartData).map((key) => ({
                  x: chartData[key].x,
                  y: chartData[key].y,
                  name: chartData[key].name,
                  visible: chartData[key].visible,
                  type: "scatter",
                }));
      
                const layout = Object.assign(
                  {},
                  baseLayout,
                  isDarkMode ? darkModeStyles : lightModeStyles,
                  {
                    title: `${data.stock_ticker} Price Line Chart`,
                  }
                );
      
                switchChartBtn.classList.remove("hidden");
                candlestickChartContainer.classList.add("hidden");
                regularChartContainer.classList.remove("hidden");
                switchChartBtn.textContent = "Switch to Candlestick Chart";
      
                Plotly.newPlot("chart-container", traces, layout, config);
```

This is one of how Plotly library is effectively utlized to visualize the financial data and technical indicators in the NATT Investment Analyst Buddy. 
