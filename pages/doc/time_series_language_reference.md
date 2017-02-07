---
title: Time Series Query Language Reference
keywords: query language
tags: [query_language]
sidebar: doc_sidebar
permalink: time_series_language_reference.html
summary: This is a quick reference for the Wavefront time series query language.
---

## Query Elements
<table width="100%">
<colgroup>
<col width="20%" />
<col width="80%" />
</colgroup>
<thead>
<tr class="header">
<th>Term</th>
<th>Definition</th>
</tr>
</thead>
<tbody>
<tr>
<td><span style="color: #4fcddc">metric</span></td>
<td><span style="color: #303030">The name of a metric.</span>
<span style="color: #303030">Example: <span style="color: #4fcddc">cpu.load.metric<span style=" color: #303030;">.</span></span></span></td>
</tr>
<tr>
<td><span style="color: #e23d39">source</span></td>
<td><span>A string sent with a metric to identify the entity that sent the metric. Sources are identified with the keyword <span style="color: #e23d39">source</span>.</span>
<span>Example: <span style="color: #e23d39">source=appServer15<span style=" color: #303030;">.</span></span></span></td>
</tr>
<tr>
<td><span style="color: #2873ee">source tag</span></td>
<td>A type of source metadata. Source tags group sources together allowing more concise queries. Source tags are identified by the keyword <span style="color: #2873ee">tag</span>.
Example: <span style="color: #2873ee">tag=app</span>.</td>
</tr>
<tr>
<td><span style="color: #3a0699">point tag</span></td>
<td><span>A type of custom metric metadata. Point tags have keys and values. <span>Example: </span><span style="color: #3a0699;">region=us-west-2b</span>.</span></td>
</tr>
<tr>
<td><span style="color: #909090;">timeWindow</span></td>
<td><span style="color: #303030">A window of time <span style="color: #000000">specified in seconds, minutes, hours, days or weeks (</span><span style="color: #909090">1s</span><span style="color: #000000">, </span><span style="color: #909090">1m</span><span style="color: #000000">, </span><span style="color: #909090">1h</span><span style="color: #000000">, </span><span style="color: #909090">1d</span><span style="color: #000000">, </span><span style="color: #909090">1w</span><span style="color: #000000">). If the unit is not specified, the default is minutes.</span></span>
<span style="color: #909090"><span style="color: #303030;">Example:</span>1h<span style="color: #303030;">.</span></span></td>
</tr>
</tbody>
</table>

## Expressions
<ul>
<li><span>ts() expression - </span>Returns all time series that match a metric name, filtered by sources, source tags, and point tags.</li>
<ul>
<li><span>Syntax:</span> ts(<span style="color: #4fcddc;">&lt;metricName&gt;</span>, [[<span style="color: #e23d39;">source=&lt;sourceName&gt;</span>] [and|or] [<span style="color: #2873ee;">tag=&lt;sourceTagName&gt;</span>] [and|or] [<span style="color: #3a0699;">&lt;<span>pointTagKey&gt;</span></span>=&lt;pointTagValue&gt;]...])</li>
<li>  <span>For metric, source, source tag, and point tag naming conventions, see <a href="javascript:;" class="jive_macro jive_macro_document">Wavefront Data Format</a>​.</span></li>
<li>  <span>Sources, source tags, and point tags are optional. For example, to return all sources sending the <span><span style="color: #4fcddc;">my.metric</span></span> metric, specify </span>ts(<span style="color: #4fcddc;">my.metric</span>).</li>
</ul>
<li>constant - A number such as 5.01, 10000, or 40. Constants can be plotted by themselves and composed in <span style="color: #3a0699;">expressions</span> using arithmetic operators.</li>
<ul>
<li markdown="span"><span>[SI prefixes](https://en.wikipedia.org/wiki/Metric_prefix) -</span>.<span style=" color: #000000;">(k, M, G, T, P, E, Z, Y) Scale constants by multiples of 1000. For example, instead of typing 1000000, type 1M, and instead of typing 7200, type 7.2k. Other common prefixes are G and T (for a billion and a trillion, useful when working with network and I/O metrics).</li>
<li>   <span>Examples: 100 -ts(<span style="color: #4fcddc;">cpu.load.1m</span>),ts(1)</span></li>
</ul>
<li><span>wildcard- <span style="color: #303030;">Matches strings</span></span> in metric names, sources, source tags, and point tags. A wildcard is represented with a "&#42;" character.  Using a catch-all wildcard (example: pointTag="&#42;") when filtering by point tags yields only time series that have this point tag present (with any value), and time series that don't have this point tag ("null") are filtered out. To find time series without a specific point tag, use the not pointTag="&#42;" construct.</li>
<ul>
<li><span>Example: <span style="color: #e23d39;">source = app-1&#42;</span> matches all sources starting with <span style="color: #e23d39;">app-1</span>: <span style="color: #e23d39;">app-10</span>, <span style="color: #e23d39;">app-11</span>, <span style="color: #e23d39;">app-12</span>, etc.</span></li>
</ul>
<li><span style="color: #3a0699;">expression</span> - A ts() expression, constant, or arithmetic or Boolean combination of a ts() expressions and constants.</li>
</ul>

## Operators
<ul>
<li>
<span class="wysiwyg-font-size-large">Boolean operators</span><span class="wysiwyg-font-size-large"> - combine ts() expressions and constants and</span><span class="wysiwyg-font-size-large"> the filtering performed by sources, source tags, and point tags.</span></li>
<ul>
<li>  <span>and</span>: Returns 1 if both arguments are nonzero. Otherwise, returns 0.</li>
<li><span><span>or</span>:</span>Returns 1 if at least one argument is nonzero. Otherwise, returns 0.</li>
<li><span>[and], [or]</span>:<span> Performs <span>strict <span>'inner join'</span></span> versions of the Boolean operators. <span>Strict operators match metric|source|point tag combinations on both sides of the operator while leaving out the non-matched ones in the result.</span></span></li></ul>
<li><span class="wysiwyg-font-size-large">Arithmetic operators</span>
</li>
<ul><li><span>+, -, \*, /: Matches metric, source, and point tag combinations on both sides of an <span style="color: #3a0699;">expression</span>. If either side of the <span style="color: #3a0699;">expression </span>is a 'singleton' -- that is, a single metric, source, or point tag <span>combination </span>-- it automatically matches up with every element on the other side of the <span style="color: #3a0699;">expression</span>.</span></li>
<li><span>[+], [-], [\*], [/]: Performs strict 'inner join' versions of the arithmetic operators. <span>Strict operators match metric|source|point tag combinations on both sides of the operator while leaving out the non-matched ones in the result.</span></span></li></ul>
<li><span>Comparison operators</span></li>
<ul><li><span><span>&lt;, &lt;=, &gt;, &gt;=, !=, =: Returns 1 if the condition is true. Otherwise returns 0. Double equals (==) is not a supported Wavefront operator.</span></span></li>
<li><span>[&lt;], [≤], [&gt;], [≥], [=], [!=]: Performs <span>strict </span>'inner join' versions of the comparison operators. Strict operators match metric|source|point tag combinations on both sides of the operator while leaving out the non-matched ones in the result.</span></li></ul>
<li><span>Examples</span></li>
<ul>
<li>  <span>(ts(<span style="color: #4fcddc;">my.metric</span>) &gt; <span>10</span>) and (ts(<span style="color: #4fcddc;">my.metric</span>) &lt; <span>20</span>) </span>returns 1 if <span style="color: #4fcddc;"><span>my.metric</span></span> is between 10 and 20. Otherwise, returns 0.</li>
<li>  <span>ts(<span style="color: #4fcddc;">cpu.load.1m</span>, <span style="color: #2873ee;">tag=prod</span> and <span style="color: #2873ee;">tag=db</span>) </span>returns <span style="color: #4fcddc;">cpu.load.1m</span> for all sources tagged with both <span style="color: #2873ee;">prod</span> and <span style="color: #2873ee;">db</span>.</li>
<li> <span></span>ts(<span style="color: #4fcddc;">db.query.rate</span>, <span style="color: #2873ee;">tag=db</span> and not <span style="color: #e23d39;">source=db5.wavefront.com</span>) returns <span style="color: #4fcddc;">db.query.rate</span> for all sources tagged with <span style="color: #2873ee;">db</span>, except for the <span style="color: #e23d39;">db5.wavefront.com</span> source.</li></ul>
<li><span>For further information on operator behavior in series matching, see <a href="javascript:;" class="jive_macro jive_macro_document">Series Matching</a>​.</span></li>
</ul>

## Tags in Queries
<ul>
<li><span><span style="color: #2873ee;">Source tags</span> are a way to group sources together. For example, if you have two sources, <span style="color: #e23d39;">appServer15</span> and <span style="color: #e23d39;">appServer16</span>, you could add the source tag <span style="color: #2873ee;">app</span> to both of them to specify that they are both app servers. Source tags aid in querying by grouping sources together. You can query ts(<span style="color: #4fcddc;">cpu.load.metric</span>, <span><span style="font-weight: inherit; color: #2873ee;">tag=app</span>)</span> instead of ts(<span style="color: #4fcddc;">cpu.load.metric</span>, <span style="color: #e23d39;">source=appServer15</span> or <span style="color: #e23d39;">source=</span><span style="color: #e23d39;"><span style="font-weight: inherit; font-style: inherit; font-family: inherit;"></span>appServer16</span>). Both queries yield the same result as long as the <span style="color: #e23d39;">app</span> tag is added to <span style="color: #e23d39;">source=appServer15</span> and <span style="color: #e23d39;">source=appServer16</span>. <span>You add source tags to sources in the Browse &gt; Sources page and through the API.</span></span></li>
<li><span><span style="color: #3a0699;">Point tags</span> are an additional way to describe metrics. An example of a point tag is <span style="color: #3a0699;">region=us-west-2b</span>. Point tags are added to a point when points are ingested through the agent.</span></li>
<li><span>Example: To query a point <span style="color: #4fcddc;">cpu.load.metric</span>, source <span style="color: #e23d39;">app2</span>, and point tag <span style="color: #3a0699;">region=us-west-2b</span>, specify ts(<span style="color: #4fcddc;">cpu.load.metric</span>, <span style="color: #3a0699;">region=us-west-2b </span>and <span style="color: #e23d39;">source=app2</span><span style="color: #303030;">)</span></span>.</li></ul>

## Variables in Queries
<ul>
<li><span>Query line variables allow you to refer to a query line as a variable in a different query line within a chart. The query line variable name is based on the query line name. The query line name, which can be found directly to the left of the query field, is referenced in a separate query line as <span class="wysiwyg-color-orange110" style="color: #7ed529;">${queryLineName}</span>. For example, if you have a query line named <span class="wysiwyg-color-orange110">queryLine1</span> with ts(<span class="wysiwyg-color-purple130" style="color: #4fcddc;">requests.latency</span>) as the <span style="color: #3a0699;">expression</span>, you can enter <span class="wysiwyg-color-orange110" style="color: #7ed529;">${queryLine1}</span> in a separate query field in that single chart to reference ts(<span class="wysiwyg-color-purple130" style="color: #4fcddc;">requests.latency</span>). If a query line and dashboard variable share the same name, then the query line variable overrides the dashboard variable for that chart. The query line being referenced must be a complete expression.</span></li>

<li><span>Aliases define any ts() expression as an <span class="wysiwyg-color-orange110">alias</span><span class="wysiwyg-color-orange110"> </span>within that single query line using a SQL-style as alias. The syntax of an alias is: <span>(<span class="wysiwyg-color-blue wysiwyg-color-blue80 expvar"><span style="color: #3a0699;">expression</span> </span>as <span class="wysiwyg-color-orange110" style="color: #7ed529;">&lt;<span class="wysiwyg-color-orange110">aliasName</span>&gt;</span>)</span></span>. Assuming you use (<span class="wysiwyg-color-blue wysiwyg-color-blue80 expvar"><span style="color: #3a0699;">expression</span> </span>as <span class="wysiwyg-color-orange110" style="color: #7ed529;">myAlias</span>) when defining the <span class="wysiwyg-color-orange110">alias, </span>you reference this <span class="wysiwyg-color-orange110">alias </span>as <span class="wysiwyg-color-orange110">$myAlias</span>. You can use <span class="wysiwyg-color-orange110">$myAlias</span> multiple times in that query line, and define multiple <span class="wysiwyg-color-orange110">aliases</span> within a query line. We recommend using <span class="wysiwyg-color-orange110">alias </span>names that are three letters or longer; specifically, you can't use the SI prefixes (such as k, G, or T) as <span class="wysiwyg-color-orange110">alias</span> names, and numeric characters are allowed only at the end of the alias name ($test123 is ok, but not $1test or $test4test). <span>Example: (ts(requests.latency) as test - mavg(120m, $test)) / sqrt(mvar(120m, $test))</span>.</li>

<li><span>Dashboard variables<span style="color: #303030;"></span> are defined for a dashboard and then used within any query line in every chart associated with that dashboard. Dashboard variables can replace any string of text rather than a complete expression like query line variables and aliases. If you define <span class="wysiwyg-color-orange110">dashvar</span> at the dashboard level, you can then refer to <span class="wysiwyg-color-orange110 expression" style="color: #7ed529;">${dashvar}</span> within any query line of any chart of that dashboard. You can use both <span class="wysiwyg-color-orange110">aliases<span class="wysiwyg-color-black">,</span> query line variables<span class="wysiwyg-color-black">, </span></span>and <span class="wysiwyg-color-orange110">dashboard</span> <span class="wysiwyg-color-orange110">variables </span>in the same line; indeed, you can even use the same <span class="wysiwyg-color-orange110">variable </span>name for a dashboard and an <span class="wysiwyg-color-orange110">alias </span>(though we don't recommend it).</span> See <a href="dashboard_variables.html">Dashboard Variables</a> for additional information about all 3 types of dashboard variables (simple, list, dynamic).</li></ul>

<span id="aggregate"></span>

## Aggregate and Raw Aggregate Functions
Aggregate and raw aggregate functions provide a way to combine (aggregate) multiple series into a single series. Standard aggregate functions first interpolate the points of the underlying set of series, and then apply the aggregate function to the interpolated series. Raw aggregate functions do not interpolate the underlying series before aggregation. Raw functions aggregate data points by time buckets.

<table width="100%">
<colgroup>
<col width="33%" />
<col width="67%" />
</colgroup>
<thead>
<tr class="header">
<th>Function</th>
<th>Definition</th>
</tr>
</thead>
<tbody>
<tr>
<td>sum(<span style="color: #3a0699;">expression</span>[,metrics|sources|sourceTags|tags|<span style="color: #3a0699">&lt;pointTagKey&gt;])</span></td>
<td><span>Returns the sum of all series. If there are gaps of data in <span style="color: #3a0699;">expression</span>, it will be interpolated.</span></td>
</tr>
<tr>
<td>rawsum(<span style="color: #3a0699;">expression</span>[,metrics|sources|sourceTags|tags|<span style="color: #3a0699">&lt;pointTagKey&gt;])</span></td>
<td><span>Returns the sum of all series.</span></td>
</tr>
<tr>
<td>avg(<span style="color: #3a0699;">expression</span>[,metrics|sources|sourceTags|tags|<span style="color: #3a0699">&lt;pointTagKey&gt;])</span></td>
<td><span>Returns the average of all series.</span></td>
</tr>
<tr>
<td>rawavg(<span style="color: #3a0699;">expression</span>[,metrics|sources|sourceTags|tags|<span style="color: #3a0699">&lt;pointTagKey&gt;])</span></td>
<td><span>Returns the average of all series. If there are gaps of data in <span style="color: #3a0699;">expression</span>, it will be interpolated.</span></td>
</tr>
<tr>
<td>min(<span style="color: #3a0699;">expression</span>[,metrics|sources|sourceTags|tags|<span style="color: #3a0699">&lt;pointTagKey&gt;])</span></td>
<td><span>Returns the lowest value of all series. </span></td>
</tr>
<tr>
<td>rawmin(<span style="color: #3a0699;">expression</span>[,metrics|sources|sourceTags|tags|<span style="color: #3a0699">&lt;pointTagKey&gt;])</span></td>
<td><span>Returns the lowest value of all series. If there are gaps of data in <span style="color: #3a0699;">expression</span>, it will be interpolated.</span></td>
</tr>
<tr>
<td>max(<span style="color: #3a0699;">expression</span>[,metrics|sources|sourceTags|tags|<span style="color: #3a0699">&lt;pointTagKey&gt;])</span></td>
<td><span>Returns the highest value of all series. </span></td>
</tr>
<tr>
<td>rawmax(<span style="color: #3a0699;">expression</span>[,metrics|sources|sourceTags|tags|<span style="color: #3a0699">&lt;pointTagKey&gt;])</span></td>
<td><span>Returns the highest value of all series. If there are gaps of data in <span style="color: #3a0699;">expression</span>, it will be interpolated.</span></td>
</tr>
<tr>
<td>count(<span style="color: #3a0699;">expression</span>[,metrics|sources|sourceTags|tags|<span style="color: #3a0699">&lt;pointTagKey&gt;])</span></td>
<td><span>Returns the number of series that are reporting. </span></td>
</tr>
<tr>
<td>rawcount(<span style="color: #3a0699;">expression</span>[,metrics|sources|sourceTags|tags|<span style="color: #3a0699">&lt;pointTagKey&gt;])</span></td>
<td><span>Returns the number of series that are reporting. If there are gaps of data in <span style="color: #3a0699;">expression</span>, it will be interpolated.</span></td>
</tr>
<tr>
<td>variance(<span style="color: #3a0699;">expression</span>[,metrics|sources|sourceTags|tags|<span style="color: #3a0699">&lt;pointTagKey&gt;])</span></td>
<td><span>Returns the variance of all series. </span></td>
</tr>
<tr>
<td>rawvariance(<span style="color: #3a0699;">expression</span>[,metrics|sources|sourceTags|tags|<span style="color: #3a0699">&lt;pointTagKey&gt;])</span></td>
<td><span>Returns the variance of all series. If there are gaps of data in <span style="color: #3a0699;">expression</span>, it will be interpolated.</span></td>
</tr>
<tr>
<td>percentile(<span style="color: #e23d39;">percentileValue</span>, <span style="color: #3a0699;">expression</span>[,metrics|sources|sourceTags|tags|<span style="color: #3a0699">&lt;pointTagKey&gt;])</span></td>
<td><span>Returns the <span><span style="color: #e23d39;">percentileValue</span></span></span> value of all series.<span>Example: If <span style="color: #e23d39;">percentileValue is</span> 99, returns the 99th percentile value of all series.</span></td>
</tr>
<tr>
<td>rawpercentile(<span style="color: #e23d39;">percentileValue</span>, <span style="color: #3a0699;">expression</span>[,metrics|sources|sourceTags|tags|<span style="color: #3a0699">&lt;pointTagKey&gt;])</span></td>
<td><span>Returns the <span><span style="color: #e23d39;">percentileValue</span></span></span> value of all series. If there are gaps of data in <span style="color: #3a0699;">expression</span>, it will be interpolated.<span>Example: If <span style="color: #e23d39;">percentileValue is</span> 99, returns the 99th percentile value of all series.</span></td>
</tr>
</tbody>
</table>

### Grouping

<span>When aggregating, to group by metrics, sources, source tags, point tags, or point tag key, use the [, metrics|sources|sourceTags|tags|<span style="color: #3a0699">&lt;pointTagKey&gt;</span>] group by clause. The clause is applied after the ts() expression, separated by a comma. Examples:</span>

-   Group by metrics: sum(ts(cpu.loadavg.1m), metrics)
-   Group by sources: sum(ts(cpu.loadavg.1m), sources)
-   Group by source tags: sum(ts(cpu.loadavg.1m), sourceTags)
-   Group by all available point tag keys: sum(ts(cpu.loadavg.1m), tags)
-   Group by the region point tag key:sum(ts(cpu.loadavg.1m), region)

## Filtering and Comparison Functions
<table width="100%">
<colgroup>
<col width="33%" />
<col width="67%" />
</colgroup>
<thead>
<tr class="header">
<th>Function</th>
<th>Definition</th>
</tr>
</thead>
<tbody>
<tr>
<td>highpass(<span style="color: #eb7a3d;">expression</span>, <span style="color: #3a0699;">expression</span>[ inner])</td>
<td>Returns only the points in <span style="color: #3a0699">expression</span> above <span style="color: #eb7a3d;">expression</span>. <span style="color: #eb7a3d;">expression</span> can be a constant.</td>
</tr>
<tr>
<td>lowpass(<span style="color: #eb7a3d;">expression</span>, <span style="color: #3a0699;">expression</span>[ inner])</td>
<td>Returns only the points in <span style="color: #3a0699;">expression</span> below <span style="color: #eb7a3d;">expression</span>. <span style="color: #eb7a3d;">expression</span> can be a constant.</td>
</tr>
<tr>
<td>min(<span style="color: #eb7a3d;">expression</span>, <span style="color: #3a0699;">expression</span>)</td>
<td>Returns the lower of the two values in <span style="color: #eb7a3d;">expression</span> and <span style="color: #3a0699">expression</span>. Example: min(<span style="color: #eb7a3d;">160</span>, ts(<span style="color: #4fcddc;">my.metric</span>)) returns 160 if <span style="color: #4fcddc;">my.metric</span> is &gt; 160. If <span style="color: #4fcddc;">my.metric</span> is &lt; 160, returns the value of <span style="color: #4fcddc;">my.metric</span>.</td>
</tr>
<tr>
<td>max(<span style="color: #eb7a3d;">expression</span>, <span style="color: #3a0699;">expression</span>)</td>
<td>Returns the higher of the two values in <span style="color: #eb7a3d;">expression</span> and <span style="color: #3a0699">expression</span>. Example: max(<span style="color: #eb7a3d;">160</span>, ts(<span style="color: #4fcddc;">my.metric</span>)) returns 160 if <span style="color: #4fcddc;">my.metric</span> is &lt; 160. If <span style="color: #4fcddc;">my.metric</span> is &gt; 160, returns the value of <span style="color: #4fcddc;">my.metric</span>.</td>
</tr>
<tr>
<td>between(<span style="color: #3a0699;">expression</span>, <span style="color: #eb7a3d;">lower</span>, <span style="color: #eb7a3d;">upper</span>)</td>
<td><span>Returns 1 if <span style="color: #3a0699;">expression</span></span>.is &gt;= <span style="color: #eb7a3d;">lower</span> and &lt;= <span style="color: #eb7a3d;">upper</span>. Otherwise, returns 0.</td>
</tr>
<tr>
<td>downsample(<span style="color: #909090;">timeWindow</span>, <span style="color: #3a0699;">expression</span>)</td>
<td>Returns the values in <span style="color: #3a0699;">expression</span> occurring in each <span style="color: #909090;">timeWindow</span>. Example: downsample(<span><span style="color: #909090;">30m</span>, ts(<span style="color: #4fcddc">my.metric</span>))</span> returns the values every half hour of <span style="color: #4fcddc">my.metric</span>.</td>
</tr>
<tr>
<td>align(<span style="color: #909090;">timeWindow</span>,<span class="wysiwyg-color-orange110" style="color: #7ed529;">[mean|median|min|max|first|last|sum|count,]</span><span style="color: #3a0699;">expression</span>)</td>
<td>Returns 1 value in <span style="color: #3a0699;">expression</span> for each <span style="color: #909090;">timeWindow</span>. Example: If you were collecting data once a minute, but wanted data points to be displayed every 30 minutes (summarized by median every 30 minutes), use align(<span style="color: #909090;">30m</span>, <span class="wysiwyg-color-orange110" style="color: #7ed529;">median</span>, ts(<span style="color: #4fcddc">my.metric</span>)).</td>
</tr>
<tr>
<td>topk(<span style="color: #7ed529;">&lt;numberOfTimeSeries&gt;</span>, <span class="wysiwyg-color-cyan130" style="color: #3a0699;">expression</span>)</td>
<td><span><span>Returns the top <span style="color: #7ed529">numberOfTimeSeries</span> </span><span style="color: #303030;">series in <span class="wysiwyg-color-cyan130" style="color: #3a0699;">expression </span></span>based on the most recent data point</span>.</td>
</tr>
<tr>
<td>bottomk(<span style="color: #7ed529;">&lt;numberOfTimeSeries&gt;</span>, <span class="wysiwyg-color-cyan130" style="color: #3a0699;">expression</span>)</td>
<td><span><span>Returns the bottom <span style="color: #7ed529">numberOfTimeSeries</span> </span><span style="color: #303030">series.in <span class="wysiwyg-color-cyan130" style="color: #3a0699;">expression</span></span> based on the most recent data point</span>.</td>
</tr>
<tr>
<td>top(<span style="color: #7ed529;">&lt;numberOfTimeSeries&gt;</span>, <span class="wysiwyg-color-cyan130" style="color: #3a0699;">expression</span>)</td>
<td>Returns the top <span style="color: #7ed529">numberOfTimeSeries</span> <span style="color: #303030">series (as 1) in <span class="wysiwyg-color-cyan130" style="color: #3a0699;">expression</span></span> based on the most recent data point. All other series are displayed as 0's.</td>
</tr>
<tr>
<td>bottom(<span style="color: #7ed529;">&lt;numberOfTimeSeries&gt;</span>, <span class="wysiwyg-color-cyan130" style="color: #3a0699;">expression</span>)</td>
<td><span><span>Returns the bottom <span style="color: #7ed529">numberOfTimeSeries</span> </span> series (as 1) in <span class="wysiwyg-color-cyan130" style="color: #3a0699;">expression</span></span> based on the most recent data point. All other series are displayed as 0's.</td>
</tr>
<tr>
<td>filter(<span style="color: #3a0699;">expression [ metric|source=|tagK=]</span>)</td>
<td>Retains the specified metric|source|<span style="color: #3d3d3d;">point tag</span> in <span style="color: #3a0699;">expression</span>. No key is required to filter a metric. To <span>filter</span> a particular source or point tag, specify a key of source= or tagK= respectively. Replace tagK with the point tag key to filter. You can specify only one parameter (metric|source|point tag) per function call. To specify multiple parameters, use a filter() call for each parameter. filter is similar to retainSeries, but does not support expanding a source tag.</td>
</tr>
<tr>
<td>retainSeries(<span style="color: #3a0699;">expression [ metric|source=|tag=|tagK=]</span>)</td>
<td>Retains the specified metric|source|source tag|<span style="color: #3d3d3d;">point tag</span> in <span style="color: #3a0699;">expression</span>. <span>No key is required to retain a metric. </span>To retain a particular source, source tag, or point tag, specify a key of source=, tag=, or tagK= respectively. Replace tagK with the point tag key to retain. You can specify <span>only </span>one parameter (metric|source|tag|point tag) per function call. To specify multiple parameters, use a retainSeries() call for each <span>parameter</span>.</td>
</tr>
<tr>
<td>removeSeries(<span style="color: #3a0699;">expression [ metric|source=|tag=|tagK=]</span>)</td>
<td>Removes the specified <span style="color: #303030;">metric|source|source tag|point tag</span> from <span style="color: #3a0699;">expression</span>. <span>No key is required to remove a metric. </span>To remove a particular source, source tag, or point tag, specify a key of source=, tag=, or tagK= respectively. Replace <span>tagK</span> with the unique point tag key to remove. You can specify <span>only </span>one parameter (metric|source|tag|point tag) per function call. To specify multiple parameters, use a removeSeries() call for each parameter.</td>
</tr>
<tr>
<td>sample(<span style=" color: #7ed529;"><span class="wysiwyg-color-yellow120">&lt;numberOfTimeSeries&gt;</span></span>, <span style="color: #3a0699;">expression)</span></td>
<td>Returns <span style="color: #7ed529">numberOfTimeSeries</span> non-random time series based on <span style="color: #3a0699;">expression</span>. This function is deterministic, as long as the underlying set of time series stays the same. The returned values may change, e.g., if a new source starts reporting the metric. <span>You can express </span><span style="color: #7ed529;">numberOfTimeSeries</span> as a number (e.g. 10) or a percentage (e.g. 17%).</td>
</tr>
<tr>
<td>random(<span style=" color: #7ed529;"><span class="wysiwyg-color-yellow120">&lt;numberOfTimeSeries&gt;</span></span>, <span style="color: #3a0699;">expression</span>)</td>
<td>Returns <span style="color: #7ed529">numberOfTimeSeries</span> random time series based on <span style="color: #3a0699">expression</span>. <span>You can express </span><span style="color: #7ed529;">numberOfTimeSeries</span> as a number (e.g. 10) or a percentage (e.g. 17%).</td>
</tr>
<tr>
<td>limit(<span style=" color: #7ed529;"><span class="wysiwyg-color-yellow120">&lt;numberOfTimeSeries&gt;</span></span>, <span style="color: #303030;">[offsetNumber,] <em></em> </span><span style="color: #3a0699;">expression</span>)</td>
<td>Returns <span style="color: #7ed529">numberOfTimeSeries</span>time series. You can express <span style="color: #7ed529;">numberOfTimeSeries</span> as a number (e.g. 10) or a percentage (e.g. 17%).</td>
</tr>
</tbody>
</table>


## Standard Time Functions
<table width="100%">
<colgroup>
<col width="33%" />
<col width="67%" />
</colgroup>
<thead>
<tr class="header">
<th>Function</th>
<th>Definition</th>
</tr>
</thead>
<tbody>
<tr>
<td><span>rate(<span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns the per-second change of <span style="color: #3a0699;">expression</span>; should be used on monotonic counter metrics (metrics that have values that only increase). Automatically handles zero-resets in counters.</span></td>
</tr>
<tr>
<td><span>deriv(<span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns the per-second change of <span style="color: #3a0699;">expression</span>. Does not handle zero-resets, but can be used on non-counter (decreasing) metrics.</span></td>
</tr>
<tr>
<td><span>lag(<span style="color: #909090;">timeWindow</span>, <span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns an <span style="color: #3a0699;">expression</span></span> time shifted by <span style="color: #909090;">timeWindow</span>, to enable comparison of an expression with its own past behavior. Example: lag(<span style="color: #909090;">3h</span>, ts(<span class="expmetric wysiwyg-color-blue130" style="color: #4fcddc;">my.metric</span>)) returns each point from <span style="color: #3a0699;">expression</span> from 3 hours ago.</td>
</tr>
<tr>
<td><span>at(<span style="color: #909090;">timeWindow</span>, <span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns the value of the <span style="color: #3a0699;">expression</span> from <span style="color: #909090;">timeWindow</span> ago. at() only looks at a single point, and returns a flat line, across all time points. If you query at(<span style="color: #909090;">2h</span>, ts(<span><span class="expmetric wysiwyg-color-blue130" style="color: #4fcddc;">my.metric</span></span>)), you'll see the value of ts(<span><span class="expmetric wysiwyg-color-blue130" style="color: #4fcddc;">my.metric</span></span>) from 2 hours ago, even if you're looking at a chart from weeks or months ago. To associate at() with a time at the beginning or end of your current chart window, replace <span style="color: #909090;">timeWindow</span></span> <span>with </span><span style="color: #909090;">"start"</span> or <span style="color: #909090;">"end"</span>, or <span style="color: #909090;">"now".</span></td>
</tr>
<tr>
<td><span>year(<span style="color: #909090;">"timezone"</span>)</span></td>
<td><span>Returns the 4-digit year for a <span style="color: #909090;">timezone</span></span>. Sample timezones include <span style="color: #909090;">"US/Pacific"</span> and <span style="color: #909090;">"Europe/London"</span>. The list of valid <span style="color: #909090;">timezones</span> is available at <a href="http://joda-time.sourceforge.net/timezones.html">http://joda-time.sourceforge.net/timezones.html</a>.</td>
</tr>
<tr>
<td><span>month(<span style="color: #909090;">"timezone"</span>)</span></td>
<td><span>Returns the numerical month for a <span style="color: #909090;">timezone.</span></span></td>
</tr>
<tr>
<td><span>dayOfYear(<span style="color: #909090;">"timezone"</span>)</span></td>
<td><span>Returns the day (within the year) for a <span style="color: #909090;">timezone</span></span>. Always returns a value from 1 to 366.</td>
</tr>
<tr>
<td><span>day(<span style="color: #909090;">"timezone"</span>)</span></td>
<td><span>Returns the day (within the month) for a <span style="color: #909090;">timezone</span></span>. Always returns a whole number from 1 to 31.</td>
</tr>
<tr>
<td><span>weekday(<span style="color: #909090;">"timezone"</span>)</span></td>
<td><span>Returns the day (within the week) for a <span style="color: #909090;">timezone</span></span>. Always returns a whole number from 1 (Monday) to 5 (Friday) for weekdays, and 6 (Saturday) and 7 (Sunday) for weekends.</td>
</tr>
<tr>
<td><span>hour(<span style="color: #909090;">"timezone"</span>)</span></td>
<td><span>Returns the hour (within the day) for a <span style="color: #909090;">timezone</span></span>. Always returns a decimal value from 0.0 to 24.0.</td>
</tr>
<tr>
<td><span>timestamp(<span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns the timestamp associated with the reported data point of </span><span style="color: #3a0699">expression</span>. To see the entire raw value of the timestamp in the legend (in epoch seconds), hold down the shift key when you hover over a point.</td>
</tr>
<tr>
<td><span>time()</span></td>
<td><span>Returns epoch seconds for each point in time.</span></td>
</tr>
</tbody>
</table>

## Moving Window Time Functions
<table width="100%">
<colgroup>
<col width="33%" />
<col width="67%" />
</colgroup>
<thead>
<tr class="header">
<th>Function</th>
<th>Definition</th>
</tr>
</thead>
<tbody>
<tr>
<td><span>mavg(<span style="color: #909090;">timeWindow</span>, <span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns the moving average of each series<span> over <span style="color: #909090;">timeWindow</span></span>.</span>
<span>Example: <span>mavg(<span style="color: #909090;">60m</span>, ts(<span style="color: #4fcddc;">my.metric</span>)) returns, at each point, the moving average over the last <span style="color: #909090;">60 minutes</span></span> for each series in <span style="color: #3a0699;">expression</span></span>.</td>
</tr>
<tr>
<td><span>msum(<span style="color: #909090;">timeWindow</span>, <span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns the moving sum of each series <span>over <span style="color: #909090;">timeWindow</span></span></span>. <span>Don't confuse this function with mcount(), which returns the <em>number of data points</em>.</span>
<span>Example: <span>msum(<span style="color: #909090;">10m</span>, ts(<span><span style="color: #4fcddc;">my.metric</span></span>)) returns, at each point, the sum of all the points over the last <span style="color: #909090;">10 minutes</span></span> for each series in <span style="color: #3a0699;">expression</span></span>.</td>
</tr>
<tr>
<td><span>mmedian(<span style="color: #909090;">timeWindow</span>, <span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns the moving median of each series<span> over <span style="color: #909090;">timeWindow</span></span></span>.</td>
</tr>
<tr>
<td><span>mvar(<span style="color: #909090;">timeWindow</span>, <span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns the moving variance of each series over <span style="color: #909090;">timeWindow</span></span>. Example: To get the moving standard deviation, apply the sqrt function to mvar: sqrt(mvar(<span style="color: #909090;">120m</span>, ts(<span style="color: #4fcddc">my.metric</span>))).</td>
</tr>
<tr>
<td><span>mcount(<span style="color: #909090;">timeWindow</span>, <span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns the number of data points over <span style="color: #909090;">timeWindow</span></span>. <span>Don't confuse this with msum(), which returns the <em>sum of the data points</em></span>.</td>
</tr>
<tr>
<td><span>mmin(<span style="color: #909090;">timeWindow</span>, <span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns the minimum of each series over <span style="color: #909090;">timeWindow</span></span>.</td>
</tr>
<tr>
<td><span>mmax(<span style="color: #909090;">timeWindow</span>, <span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns the maximum of each series over <span style="color: #909090;">timeWindow</span></span>.</td>
</tr>
<tr>
<td><span>mpercentile(<span style="color: #909090;">timeWindow</span>, <span style="color: #e23d39;">percentileValue</span>, <span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns the <span>percentile</span> of each series over <span style="color: #909090;">timeWindow</span></span>. <span style="color: #e23d39;">percentileValue</span> must be &gt;= <span class="wysiwyg-color-orange110" style="color: #e23d39;">0</span> and &lt;= <span class="wysiwyg-color-orange110" style="color: #e23d39;">100</span>.</td>
</tr>
<tr>
<td><span>mcorr(<span class="expression" style="color: #909090;"><span>timeWindow</span></span>, <span class="expression" style="color: #3a0699;"><span>expression1</span></span>, <span><span style="color: #3a0699;">expression2</span><span class="wysiwyg-color-black expopt">[ inner]</span></span></span>)</td>
<td><span>Returns the moving correlation between two time series <span style="color: #3a0699;">expressions</span> over <span style="color: #909090;">timeWindow</span>.</span></td>
</tr>
<tr>
<td><span>integrate(<span style="color: #909090;">timeWindow</span>,<span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns the moving integration on a given time series <span style="color: #3a0699;">expression</span> over <span style="color: #909090;">timeWindow</span></span>.</td>
</tr>
<tr>
<td><span>integral(<span><span style="color: #3a0699;">expression</span><span class="wysiwyg-color-black">)</span></span></span></td>
<td><span>Returns the moving sum over time for the given time series </span><span style="color: #3a0699">expression<span style="color: #303030;"> over the time interval of the current chart window. Always starts at 0 on the left side of the chart showing the total accumulation over the time duration of the current chart window. </span></span></td>
</tr>
<tr>
<td><span>flapping(<span><span style="color: #909090;">timeWindow</span><span class="wysiwyg-color-black">,</span> <span style="color: #3a0699;">expression</span><span class="wysiwyg-color-black">)</span></span></span></td>
<td><span>Returns the number of times a counter has reset within <span style="color: #909090;">timeWindow</span></span>.</td>
</tr>
<tr>
<td><span>any(<span style="color: #909090;">timeWindow</span>, <span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns 1 if the <span style="color: #3a0699;">expression</span> over <span style="color: #909090;">timeWindow</span> has been non-zero at any time. Otherwise, it returns 0.</span></td>
</tr>
<tr>
<td><span>all(<span style="color: #909090;">timeWindow</span>, <span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns 1 if the <span style="color: #3a0699;">expression</span> over <span style="color: #909090;">timeWindow</span> has been non-zero at every time in that window. Otherwise, it returns 0.</span></td>
</tr>
</tbody>
</table>

## Conditional Functions
<table width="100%">
<colgroup>
<col width="33%" />
<col width="67%" />
</colgroup>
<thead>
<tr class="header">
<th>Function</th>
<th>Definition</th>
</tr>
</thead>
<tbody>
<tr>
<td><span>if(<span><span style="color: #3a0699;">expression</span></span>, <span style="color: #f1b259;">ThenExpression</span>, <span class="expvar wysiwyg-color-yellow130" style="color: #4fcddc;">ElseExpression</span>)</span></td>
<td><span>Returns <span><span style="color: #f1b259;">ThenExpression</span></span></span> if <span style="color: #3a0699;">expression</span> &gt;=1. Otherwise, returns <span class="expvar wysiwyg-color-yellow130" style="color: #4fcddc;">ElseExpression</span>.
<span>Example: <span> if <span style="color: #505050;">(ts(<span style="color: #4fcddc;">my.metric</span>) &gt;= 10</span>, <span style="color: #f1b259;">ts(my.metric)</span>, <span style="color: #4fcddc;">ts(another.metric)</span>)  returns </span><span style="color: #f1b259;">ts(<span>my.metric</span>) only when <span style="color: #505050;">ts(<span style="color: #4fcddc">my.metric</span>)</span></span>&gt;= 10; when <span style="color: #505050;">ts(<span style="color: #4fcddc">my.metric</span>)</span></span> &lt; 10, it returns <span style="color: #4fcddc;">ts(<span>another.metric</span>)</span>.</td>
</tr>
</tbody>
</table>

## Rounding Functions
<table width="100%">
<colgroup>
<col width="33%" />
<col width="67%" />
</colgroup>
<tbody>
<thead>
<tr class="header">
<th>Function</th>
<th>Definition</th>
</tr>
</thead>
<tr>
<td><span>round(<span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns the nearest whole number to <span style="color: #3a0699;">expression</span></span>.</td>
</tr>
<tr>
<td><span>ceil(<span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Rounds up <span style="color: #3a0699;">expression</span></span> to the next largest whole number.</td>
</tr>
<tr>
<td><span>floor(<span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Rounds down <span style="color: #3a0699;">expression</span></span> to the next smallest whole number.</td>
</tr>
</tbody>
</table>

## Missing Data Functions
<table width="100%">
<colgroup>
<col width="33%" />
<col width="67%" />
</colgroup>
<tbody>
<thead>
<tr class="header">
<th>Function</th>
<th>Definition</th>
</tr>
</thead>
<tr>
<td><span>default([<span><span style="color: #909090;">timeWindow</span>,</span>][<span style="color: #eb7a3d;">delayTime</span>,]<span class="wysiwyg-color-orange110 expvar">defaultValue</span>,<span style="color: #3a0699;">expression</span>)</span></td>
<td><span><span>Fills in gaps in <span><span style="color: #3a0699;">expression</span></span></span> with defaultValue (whether that's a constant or an expression). The optional argument (<span style="color: #909090;">timeWindow</span>) fills in that period of time after each existing point (for example, <span class="exptw"><span>5m</span> </span>for <span>5 minutes</span>); without this argument, all gaps are filled in. The optional argument (<span style="color: #eb7a3d;">delayTime</span>) refers to the amount of time that must pass without a reported value in order for the default value to be applied.</span></td>
</tr>
<tr>
<td><span>last([<span><span style="color: #909090;">timeWindow</span>,</span>]<span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Fills in gaps in <span style="color: #3a0699;">expression</span> with the last known value of <span style="color: #3a0699;">expression</span></span>. <span style="color: #909090;">timeWindow </span> fills in a specified time period after each existing point.</td>
</tr>
<tr>
<td><span>next([<span><span style="color: #909090;">timeWindow</span>,</span>]<span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Fills in gaps in <span style="color: #3a0699;">expression</span> with the next known value of <span style="color: #3a0699;">expression</span>. <span style="color: #909090;">timeWindow</span></span> fills in a specified time period before each existing point.</td>
</tr>
<tr>
<td><span>interpolate(<span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Fills in gaps in <span style="color: #3a0699;">expression</span> with a continuous linear interpolation of points.</span></td>
</tr>
</tbody>
</table>                                                                               

## Metadata Functions
<table width="100%">
<colgroup>
<col width="33%" />
<col width="67%" />
</colgroup>
<thead>
<tr class="header">
<th>Function</th>
<th>Definition</th>
</tr>
</thead>
<tbody>
<tr>
<td><span>aliasMetric(<span style="color: #3a0699;">expression</span>, [<span style="color: #eb7a3d;">metric|source|{tagk, &lt;pointTagKey&gt;}, </span>] <span style="color: #5ee697;">zeroBasedNodeIndex</span> [ <span style="color: #909090;">delimiterDefinition</span>])</span></td>
<td><span>Extracts a string from a <span style="color: #eb7a3d;">metric</span>, <span style="color: #eb7a3d;">source</span>, or <span style="color: #eb7a3d;">point tag value</span> from the result of <span style="color: #3a0699;">expression</span>, and uses that extracted string to rename the metric in <span style="color: #3a0699;">expression</span>. In the example below, the extracted string is based on a <span style="color: #5ee697;">zeroBasedNodeIndex</span>. For example, using the metric cpu.loadavg.1m, to get loadavg, you would use the argument <span style="color: #5ee697;">1</span> for <span style="color: #5ee697;">zeroBasedNodeIndex</span>. To get cpu, you would use the argument <span style="color: #5ee697;">0</span>. By default, the <span style="color: #5ee697;">zeroBasedNodeIndex</span> assumes dot (.) delimiters in the metric name. If the delimiter is a character other than a dot, use the optional <span style="color: #909090;">delimiterDefinition</span>. For example, if the metric name is cpu-loadavg-1m, then use the <span style="color: #909090;">delimiterDefinition</span> <span style="color: #909090;">,&quot;-&quot;</span>. Additionally, you can replace <span style="color: #5ee697;">zeroBasedNodeIndex</span>/<span style="color: #909090;">delimiterDefinition</span> with a regular expression.</span>
<span>Example: </span>
<ul>
<li><span>aliasMetric(ts(</span><span style="color: #3a0699">cpu.load.1m, source, </span><span style="color: #5ee697">1</span>.</li>
<li><span><span>aliasMetric(ts(</span><span style="color: #3a0699">database-storage-disk1, source, </span><span style="color: #5ee697">2, <span style="color: #909090"><em>-</em></span></span>.</span></li>
</ul></td>
</tr>
<tr>
<td><span>aliasSource(<span style="color: #3a0699;">expression</span>, [<span style="color: #eb7a3d;">metric|source|{tagk, <span style="color: #eb7a3d">&lt;pointTagKey&gt;</span><span style="color: #eb7a3d">}</span>,</span>] <span style="color: #5ee697;">zeroBasedNodeIndex</span> [ <span style="color: #909090;">delimiterDefinition</span>])</span></td>
<td><span>Extracts a string from a <span style="color: #eb7a3d;">metric</span>, <span style="color: #eb7a3d;">source</span>, or <span style="color: #eb7a3d;">point tag value</span> from the result of <span style="color: #3a0699;">expression</span>, and uses that extracted string to rename the source in <span style="color: #3a0699;">expression</span>. In the example above, the extracted string is based on a <span style="color: #5ee697;">zeroBasedNodeIndex</span>. For example, using the source host1.app.az, to get app, you would use the argument <span style="color: #5ee697;">1</span> for <span style="color: #5ee697;">zeroBasedNodeIndex</span>. To get host1, you would use the argument <span style="color: #5ee697;">0</span>. By default, <span style="color: #5ee697;">zeroBasedNodeIndex</span> assumes dot (.) delimiters in the source name. If the delimiter is a character other than a dot, use the optional <span style="color: #909090;">delimiterDefinition</span>. For example, if the source is host1-app-az, then use the <span style="color: #909090;">delimiterDefinition</span> <span style="color: #909090;">,&quot;-&quot;</span>. <span>Additionally, you can replace </span><span style="color: #5ee697;">zeroBasedNodeIndex</span>/<span style="color: #909090;">delimiterDefinition</span> with a regular expression.</span></td>
</tr>
<tr>
<td><span>taggify(<span style="color: #3a0699;">expression</span>, <span style="color: #eb7a3d;">metric|source|{tagk, <span style="color: #eb7a3d">&lt;pointTagKey&gt;</span><span style="color: #eb7a3d">}</span></span>, <span style="color: #4fcddc;">&lt;newPointTagKey&gt;</span>, <span style="color: #5ee697;">zeroBasedNodeIndex</span> [ <span style="color: #909090;">,delimiterDefinition</span>])</span></td>
<td><span>Extracts <span>a string</span> from a </span><span style=" color: #eb7a3d;">metric, </span><span style=" color: #eb7a3d;">source, or </span><span style=" color: #eb7a3d;">point tag value</span> <span>from the result of </span><span style=" color: #3a0699;">expression</span>, and uses that extracted <span>string</span> to create a synthetic point tag. <span>In the example above, the extracted string is based on a </span><span style="color: #5ee697;">zeroBasedNodeIndex</span>. For example, using the point tag microservice.app, to get app, you would use the argument <span style="color: #5ee697">1 for </span><span style="color: #5ee697;">zeroBasedNodeIndex</span>. To get microservice, you would use the argument <span style="color: #5ee697;">0</span>. By default, <span style="color: #5ee697;">zeroBasedNodeIndex</span> assumes dot (.) delimiters in the point tag name. If the delimiter is a character other than a dot, use the optional <span style="color: #909090;">delimiterDefinition</span>. For example, if the tag name is microservice-app, then use the <span style="color: #909090;">delimiterDefinition</span> <span style="color: #909090;">,&quot;-&quot;</span>. <span>Additionally, you can replace </span><span style="color: #5ee697;">zeroBasedNodeIndex</span>/<span style="color: #909090;">delimiterDefinition</span> with a regular expression.</td>
</tr>
</tbody>
</table>

## Exponential and Trigonometric Functions
<table width="100%">
<colgroup>
<col width="33%" />
<col width="67%" />
</colgroup>
<thead>
<tr class="header">
<th>Function</th>
<th>Definition</th>
</tr>
</thead>
<tbody>
<tr>
<td><span>sqrt(<span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns the square root of <span style="color: #3a0699;">expression</span></span>.</td>
</tr>
<tr>
<td><span>pow(<span style="color: #3a0699;">expression</span>, <span style="color: #f1b259;">expression</span>[, inner]</span>)</td>
<td><span>Raises <span style="color: #3a0699;">expression</span> to the power of </span><span style=" color: #f1b259;"><span>expression</span></span>. Wavefront does not support imaginary numbers, so pow(-1, 0.5) returns no data.</td>
</tr>
<tr>
<td><span>exp(<span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns the exponential of <span style="color: #3a0699;">expression</span></span>.</td>
</tr>
<tr>
<td><span>log(<span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns the natural log of <span style="color: #3a0699;">expression</span></span>.</td>
</tr>
<tr>
<td><span>log10(<span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns the log base 10 of <span style="color: #3a0699;">expression</span></span>.</td>
</tr>
<tr>
<td><span>toDegrees(<span class="wysiwyg-color-blue120 expvar">numRadians</span>), toRadians(<span class="wysiwyg-color-blue120 expvar">numDegrees</span>)</span></td>
<td><span>Convert radians to degrees, and vice versa.</span></td>
</tr>
<tr>
<td>sin(<span style="color: #3a0699;">expression</span>),cos(<span style="color: #3a0699;">expression</span>),tan(<span style="color: #3a0699;">expression</span>), asin(<span style="color: #3a0699;">expression</span>),acos(<span style="color: #3a0699;">expression</span>),atan(<span style="color: #3a0699;">expression</span>),atan2(<span style="color: #3a0699;">expression</span>), sinh(<span style="color: #3a0699;">expression</span>), cosh(<span style="color: #3a0699;">expression</span>), tanh(<span style="color: #3a0699;">expression</span>)</td>
<td><span>Performs the specified trigonometric function on <span style="color: #3a0699;">expression</span></span> interpreted in radians.</td>
</tr>
</tbody>
</table>

## Event Functions
<table width="100%">
<colgroup>
<col width="33%" />
<col width="67%" />
</colgroup>
<thead>
<tr class="header">
<th>Function</th>
<th>Definition</th>
</tr>
</thead>
<tbody>
<tr>
<td><span>events(<span style="; font-size: 12pt;"><span style="color: #2873ee;">filters</span></span>)</span></td>
<td>Displays events that match <span style="color: #2873ee;">filters</span> on a chart. The chart must contain at least 1 ts() query for events to display. The result of the events() function can be passed as an argument to functions that accept events. The available filters are <a href="#event_filters">Event Filters</a>.</td></tr>
<tr>
<td><span>count(<span style="color: #3a0699;">events</span>)</span></td>
<td><span>Converts <span style="color: #3a0699;">events</span> into a single time series, where every data point represents the number of events that started at that time minus the number of events that ended at that time. Instantaneous events are represented as a single &quot;0&quot; value: 1 started minus 1 ended (instantaneous events are defined as events having their end time equal to their start time).</span></td>
</tr>
<tr>
<td><span>ongoing(<span><span style="color: #3a0699;">events</span></span>)</span></td>
<td><span style="color: #000000">Returns a continuous time series representing the number of ongoing events at any given moment.</span></td>
</tr>
<tr>
<td><span>closed(<span><span style="color: #3a0699;">events</span></span>)</span></td>
<td><span><span style="color: #000000;">Returns events that have ended and </span><span style="color: #000000;">instantaneous events that occurred in the past.</span></span></td>
</tr>
<tr>
<td><span>until(<span><span style="color: #3a0699;">events</span></span>)</span></td>
<td><span style="color: #000000">Returns synthetic events that start at the beginning of time (Jan 1, 1970) and end where the input events start.</span></td>
</tr>
<tr>
<td><span>after(<span><span style="color: #3a0699;">events</span></span>)</span></td>
<td><span style="color: #000000">Returns synthetic ongoing events that start the moment the input events end.</span></td>
</tr>
<tr>
<td><span>since(<span style="color: #3a0699;">events</span>)</span></td>
<td><span style="color: #000000">Returns synthetic events with the same start time and no end time (converts all input events to ongoing).</span></td>
</tr>
<tr>
<td><span>since(<span style="color: #909090">timeWindow</span>)</span></td>
<td><span style="color: #000000">Creates a single synthetic event that started </span><span style="color: #909090">timeWindow</span><span style="color: #000000"> ago and ended &quot;now&quot;. </span><span style="color: #909090">timeWindow</span><span style="color: #000000"> can be specified in seconds, minutes, hours, days or weeks (e.g., <span style="color: #909090;">1s</span>, <span style="color: #909090;">1m</span>, <span style="color: #909090;">1h</span>, <span style="color: #909090;">1d</span>, <span style="color: #909090;">1w</span>). If the unit is not specified, the default is minutes.</span></td>
</tr>
<tr>
<td><span>timespan(<span style="color: #eb7a3d">startTimestamp, endTimestamp</span>)</span></td>
<td><span style="color: #000000">Creates a single synthetic event with the specified start and end timestamps. A timestamp can be expressed in epoch seconds or using a time expression such as "5 minutes ago". Example: timespan("5 minutes ago", "2 minutes ago").</span></td>
</tr>
<tr>
<td><span>first(<span style="color: #3a0699;">events</span>)</span></td>
<td><span style="color: #000000">Returns a single event with the earliest start time.</span></td>
</tr>
<tr>
<td><span>last(<span style="color: #3a0699;">events</span>)</span></td>
<td><span style="color: #000000; font-size: 12pt;"><span style="color: #000000">Returns</span> a single event with the latest start time.</span></td>
</tr>
<tr>
<td><span>firstEnding(<span style="color: #3a0699;">events</span>)</span></td>
<td><span><span style="color: #000000">Returns</span> <span style="color: #000000;">a single event with the earliest end time.</span></span></td>
</tr>
<tr>
<td><span>lastEnding(<span style="color: #3a0699;">events</span>)</span></td>
<td><span style="color: #000000"><span style="color: #000000">Returns</span> a single event with the latest end time.</span></td>
</tr>
</tbody>
</table>

<a name="event_filters"/>

### Event Filters
<table width="100%">
<colgroup>
<col width="13%" />
<col width="53%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th>Filter</th>
<th>Description</th>
<th>Example Filter</th>
</tr>
</thead>
<tbody>
<tr>
<td><span>alertId</span></td>
<td><span>The ID of the alert that created the event.</span></td>
<td><span>alertID=1411189741192</span></td>
</tr>
<tr>
<td><span>alertTag</span></td>
<td><span>A tag associated with the alert that generated the event.</span></td>
<td><span>alertTag=MicroService.App1.&#42;</span></td>
</tr>
<tr>
<td><span>created</span></td>
<td><span>The ID of the alert that created the event. This filter is deprecated. Use alertID instead.</span></td>
<td></td>
</tr>
<tr>
<td><span>eventTag</span></td>
<td><span>A tag associated with the event.</span></td>
<td><span>eventTag="codepushes"</span></td>
</tr>
<tr>
<td><span>host</span></td>
<td><span>The name of the host associated with the alert that generated the event. This filter is deprecated. Use source instead.</span></td>
<td></td>
</tr>
<tr>
<td><span>name</span></td>
<td><span>An event name.</span></td>
<td>name="Jan 2017 code push"</td>
</tr>
<tr>
<td><span>severity</span></td>
<td><span>The classification of the user event or the severity of alert that generated the event. User event classification levels are severe,warn,info, and unclassified. Although an event can be classified as unclassified, the severity parameter does not accept unclassified as a valid value.  Alert severity levels are severe,warn,smoke,info.</span></td>
<td>severity=info</td>
</tr>
<tr>
<td><span>source or tag</span></td>
<td><span>The source or source tag associated with the alert that generated the event. The source parameter allows you to display events generated by an alert based on a single source or set of sources. The tag parameter works the same way, but allows you to specify a source tag instead of a source.</span></td>
<td>source=06bef3e0d35a</td>
</tr>
<tr>
<td><span>subtype</span></td>
<td><span>An event subtype: <span>failing</span> or <span>recovered.</span></span></td>
<td>subtype=failing</td>
</tr>
<tr>
<td><span>target</span></td>
<td><span>A notification target for the alert that generated the event.</span></td>
<td><span>target=&quot;mailto:ben@example.com&quot;</span></td>
</tr>
<tr>
<td><span>type</span></td>
<td><span>An event type:<span>alert</span> or <span>alert-detail.</span></span></td>
<td><span>type=alert</span></td>
</tr>
</tbody>
</table>

<span>Example: events(type=alert, name=&quot;disk space is low&quot;, alertTag=MicroService.App1.\*) For more information, see <a href="javascript:;" class="jive_macro jive_macro_document">Using events() Queries in Wavefront</a> and <a href="javascript:;" class="jive_macro jive_macro_document">Advanced event() expressions.</a></span>

## <span id="misc"></span>Miscellaneous Functions
<table width="100%">
<colgroup>
<col width="33%" />
<col width="67%" />
</colgroup>
<thead>
<tr class="header">
<th>Function</th>
<th>Definition</th>
</tr>
</thead>
<tbody>
<tr>
<td><span>collect(<span style="color: #3a0699;">expression</span>, <span style="color: #3a0699;">expression2</span>, <span style="color: #3a0699;">expression3</span>, ...)</span></td>
<td><span>Returns two or more ts() expressions combined into a single ts() expression. Each expression includes an synthetic collect_&lt;number&gt; point tag, where &lt;number&gt; is the number of expressions.</span></td>
</tr>
<tr>
<td><span>exists(<span style="color: #3a0699;">expression</span>)</span></td>
<td><span><span>Returns 1 i</span>f at least one value in </span><span style=" color: #3a0699;">expression</span> has been reported in the last 4 weeks. Otherwise, it returns 0.</td>
</tr>
<tr>
<td><span>abs(<span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns the absolute value of </span><span style="color: #3a0699">expression<span style="color: #303030;">.</span></span></td>
</tr>
<tr>
<td><span>random()</span></td>
<td><span>Returns random values between 0.0 and 1.0. If you reload a chart that uses random(), the reloaded chart returns new random values.</span></td>
</tr>
<tr>
<td><span>normalize(<span style="color: #3a0699;">expression</span>)</span></td>
<td><span>Returns every series in </span><span style="color: #3a0699">expression</span> scaled so it has a minimum of 0 and a maximum of 1.0.</td>
</tr>
<tr>
<td><span>haversine(<span style="color: #3a0699;">expression1</span>, <span style="color: #3a0699;">expression2</span>, <span style="color: #3a0699;">expression3</span>, ...)</span></td>
<td><span>Returns the distance between coordinates. <span style="color: #3a0699;">expression</span><span style="color: #3a0699;">(s)</span> can be constants or ts() expressions.</span></td>
</tr>
</tbody>
</table>

{% include links.html %}
