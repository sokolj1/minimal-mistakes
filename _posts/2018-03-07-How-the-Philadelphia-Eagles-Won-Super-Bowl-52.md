---
header:
  image: /assets/foles.jpg
values:
      comments: true
---

# A statistical breakdown of the birdgang's success in the 2017-2018 season

As a lifelong Giants fan, it was painful to watch the Philadelphia Eagles win their first championship 
since 1960, and their first Super Bowl victory in the history of the franchise. Oh well, gone are the days of using the default end all argument with the hypothetical question “How many Super Bowl rings do the Eagles have?” It makes me sick to my stomach.

Although the Eagles are not my favorite NFL team in the world (ranked 31/32 of my favorite teams right in front of Dallas), from an objective standpoint, the Philadelphia Eagles performed at an outstanding level this past season. They ended the season with a 13-3 win/loss record. The birdgang also cliched the NFC East division title by week 14, but this seemed like a pyrric victory with the loss of QB Carson Wentz to a torn ACL and LCL. With backup quarterback Nick Foles now at the helm, even the media inside the city of Philadelphia wrote this team off as possibly losing the divisional round game. As for the fans of the other 31 NFL teams, they doubted the Eagle's ability to sustain their level of success.

So how did this team manage to win a Super Bowl against the invincible New England Patriots featuring an offensive shootout that totaled over 1000 passing yards? In the words of Jason Kelce, 
“underdogs are hungry dogs.” Our main focus is diving into the regular season statistics comparing the 2016 to 2017 season to observe which facets of the team markedly improved over the course of one offseason. 
We'll also look at the magic carpet ride of their playoff run, and how this obstensibly ragtag team beat Tom Brady on the football world's biggest stage in Super Bowl LII. 
The power of data visualization will shed light on the Philadelphia Eagle’s rise to power from last place in the NFC East Division in 2016 go one of the best teams in pro football one year later.


<style>
	.links line {
  	stroke: #999;
  	stroke-opacity: 0.6;
	}
	.nodes circle {
  	stroke: #fff;
  	stroke-width: 1.5px;
	}
	</style>
	<svg width="900" height="480" font-family="sans-serif" font-size="10" text-anchor="middle"></svg>
	<script src="https://d3js.org/d3.v4.min.js"></script>
	<script>
		var svg = d3.select("svg"),
    		margin = {top: 20, right: 20, bottom: 30, left: 20},
    		width = +svg.attr("width") - margin.left - margin.right,
    		height = +svg.attr("height") - margin.top - margin.bottom,
    		g = svg.append("g").attr("transform", "translate(" + margin.left + "," + margin.top + ")");
		var x0 = d3.scaleBand()
    			.rangeRound([0, width])
    			.paddingInner(0.1);
		var x1 = d3.scaleBand()
    			.padding(0.05);
		var y = d3.scaleLinear()
    			.rangeRound([height, 0]);
		var z = d3.scaleOrdinal()
    			.range(["#98abc5", "#8a89a6", "#7b6888", "#6b486b", "#a05d56", "#d0743c", "#ff8c00"]);
		d3.csv("SB_data_viz_1.csv", function(d, i, columns) {
  		for (var i = 1, n = columns.length; i < n; ++i) d[columns[i]] = +d[columns[i]];
  			return d;
		}, function(error, data) {
  			if (error) throw error;
  		var keys = data.columns.slice(1);
  		x0.domain(data.map(function(d) { return d.State; }));
  		x1.domain(keys).rangeRound([0, x0.bandwidth()]);
  		y.domain([0, d3.max(data, function(d) { return d3.max(keys, function(key) { return d[key]; }); })]).nice();
  		g.append("g")
    		.selectAll("g")
    		.data(data)
    		.enter().append("g")
      		.attr("transform", function(d) { return "translate(" + x0(d.State) + ",0)"; })
    		.selectAll("rect")
    		.data(function(d) { return keys.map(function(key) { return {key: key, value: d[key]}; }); })
    		.enter().append("rect")
      		.attr("x", function(d) { return x1(d.key); })
      		.attr("y", function(d) { return y(d.value); })
      		.attr("width", x1.bandwidth())
      		.attr("height", function(d) { return height - y(d.value); })
      		.attr("fill", function(d) { return z(d.key); });
  		g.append("g")
      		.attr("class", "axis")
      		.attr("transform", "translate(0," + height + ")")
      		.call(d3.axisBottom(x0));
  		g.append("g")
      		.attr("class", "axis")
      		.call(d3.axisLeft(y).ticks(null, "s"))
    		.append("text")
      		.attr("x", 2)
      		.attr("y", y(y.ticks().pop()) + 0.5)
      		.attr("dy", "0.32em")
      		.attr("fill", "#000")
      		.attr("font-weight", "bold")
      		.attr("text-anchor", "start")
      		.text("");
  		var legend = g.append("g")
      		.attr("font-family", "sans-serif")
      		.attr("font-size", 10)
      		.attr("text-anchor", "end")
    		.selectAll("g")
    		.data(keys.slice().reverse())
    		.enter().append("g")
      		.attr("transform", function(d, i) { return "translate(0," + i * 20 + ")"; });
  		legend.append("rect")
      		.attr("x", width - 19)
      		.attr("width", 19)
      		.attr("height", 19)
      		.attr("fill", z);
  		legend.append("text")
      		.attr("x", width - 24)
      		.attr("y", 9.5)
      		.attr("dy", "0.32em")
      		.text(function(d) { return d; });
		});
	</script>
