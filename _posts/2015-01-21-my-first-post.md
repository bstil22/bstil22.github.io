---
layout: post
title:  Stockcharts with Highcharts and the Quandl API
---
## Setup
For this tutorial, we will be using Quandl to fetch the historical stock data and
highcharts to display the data. I chose the WIKI data set because it was free and
very simple to use.

The first thing we need to do is create an html file to link to the javascript libraries we will be using.
I also linked to a stylesheet which I named stock.css.
{% highlight html %}
<!DOCTYPE HTML>
<head>
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>
<script src="http://code.highcharts.com/stock/highstock.js"></script>
<script src="http://code.highcharts.com/stock/modules/exporting.js"></script>
<link rel="stylesheet" type="text/css" href="stock.css">
</head>
{% endhighlight %}

Now that we have the basics set up, lets create a form that allows our application
to accept user input.
{% highlight html %}
<body>
<h4>Enter Quote</h4>
<input type="text" id="stock" placeholder="Ex. AAPL"  />
<p id="error"></p>
<button type="submit" id="symbol">Submit</button>
<div id="price"></div>
<div id="time"></div>

<script src="stock.js" charset="utf-8"></script>
<div id="container"></div>
</body>

</html>
{% endhighlight %}
Notice that I linked to a javascript file named stock.js. In your stock.js file,
add the below code. Notice that the setup function simply fetches the data
and formats it.
{% highlight js %}
setup = function() {
  $("#symbol").click(function() {
    var stockQuote = $("#stock").val();
    var req = $.get("https://www.quandl.com/api/v1/datasets/WIKI/" +
    stockQuote + ".json?auth_token=LnZgCF8fyVmHREMm9p3a")
    .success(function(data) {
      stockTimeSeries = []

      for (i = 0; i < data.data.length; i++) {
        stockPrice = data.data[i][4]
        date = Date.parse(data.data[i][0])
        stockTimeSeries.unshift([date, stockPrice])
      }

      renderChart(stockQuote, stockTimeSeries)
      })
      .error(function(data) {
        $("#error").html("Please enter a valid symbol.");
        })
        });
      }
{% endhighlight %}
Now that we have our data formatted properly for highcharts charting library, lets
add the proper code to create the stock charts. But before we add the charting functionality,
we need to create our stock.css file.
{% highlight css %}
#container {
  width: 600px;
  height: 100%;
  text-align: center;
  margin: 0 auto;
}
{% endhighlight %}
Okay, the width and height of the chart are now setup, now lets add the code to
generate the graph.
{% highlight js %}
function renderChart(title, timeSeries) {
  // Create the chart
  $('#container').highcharts('StockChart', {
    rangeSelector: {
      selected: 1
      },
      // xAxis: {
        // 	type: "datetime", //add this line
        // },

        title: {
          text: title
          },

          series: [{
            name: title,
            data: timeSeries,
            tooltip: {
              valueDecimals: 2
            }
            }]
            })
          }

          setup();
{% endhighlight %}
That's all she wrote. Open it up in the browser and check it out!
