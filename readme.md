**Visual** is a Javascript library for data visualization developed by the **Statistical Institute of Catalonia ([Idescat](https://www.idescat.cat/en/))**. It is based on popular open source solutions. **Visual** offers a simple interface that encapsulates the complexity of these solutions for the most common chart types.

**Not a web developer?** Don't worry: you can try Visual online by simply filling in fields in the [Visual Maker](https://idescat.github.io/visual/).

* [Supported visualizations](#supported-visualizations)
* [Test](#test)
* [Configuration](#configuration)
* [Installation](#installation)
* [The Visual Object](#the-visual-object)
* [Maps](#maps)
* [Public functions](#public-functions)
* [Dependencies](#dependencies)
* [Known limitations](#known-limitations)
* [How to contribute](#how-to-contribute)

# Supported visualizations

**Visual** currently supports the following visualizations:

* Distribution of a categorical variable (vertical bar chart): *[bar](#bar-properties)*
* Ranking (horizontal bar chart): *[rank](#rank-properties)*
* Pie (circle chart): *[pie](#pie-properties)*
* Stacked/non-stacked time series (vertical bar chart): *[tsbar](#tsbar-properties)*
* Time series (line chart): *[tsline](#tsline-properties)*
* Population pyramid: *[pyram](#pyram-properties)*
* Scatter plot (XY chart): *[xy](#xy-properties)*
* Choropleth map: *[cmap](#cmap-properties)*

# Test

Download the [full source and tests](https://github.com/idescat/visual/archive/master.zip) and then start your browser and try the examples in the [test folder](https://github.com/idescat/visual/tree/master/test). Or play with the [Visual Viewer](https://github.com/idescat/visual/tree/master/viewer/index.html) included in the package. Or build your own charts and store them online with the [Visual Maker](https://idescat.github.io/visual/).

# Configuration

Edit [visual.setup.js](https://github.com/idescat/visual/blob/master/visual.setup.js) and [visual.css](https://github.com/idescat/visual/blob/master/visual.css) to meet your needs. You might need to provide your own maps and adapt the *VisualJS.func.legend* function inside [visual.maps.js](https://github.com/idescat/visual/blob/master/maps/visual.maps.max.js) (see the [maps folder](https://github.com/idescat/visual/tree/master/maps)).

# Installation

**Visual** allows three running modes: **webpage** (recommended), **simple** and **manual**. See the examples in the [test folder](https://github.com/idescat/visual/tree/master/test).

**Not ready to install?**  You can try Visual online: visit the [Visual Maker](https://idescat.github.io/visual/).

### Webpage mode

In webpage mode, the visualization is the only content on an html5 page. Use the [webpage template](https://github.com/idescat/visual/blob/master/templates/webpage.html) to build your page. To embed the visualization, use an iframe pointing to your page ([example](https://github.com/idescat/visual/blob/master/test/index.html)).

### Simple mode

In simple mode, the visualization is embedded in a page using the [VisualJS.iframe](#visualjsiframe) function. Use the [simple template](https://github.com/idescat/visual/blob/master/templates/simple.html) to build your page. To embed the visualization, include the visual.js and visual.setup.js files and use a script tag with a unique *id* and invoke [VisualJS.iframe](#visualjsiframe) passing a [Visual Object](#the-visual-object) (with the same *id* as the script tag) and a CSS file (or CSS rules) ([example](https://github.com/idescat/visual/blob/master/test/simple.html)).

### Manual mode

In manual mode, the visualization is directly embedded in a page. Use the [manual template](https://github.com/idescat/visual/blob/master/templates/manual.html) as an example. If you are embedding a single visualization, include the same javascripts as in the webpage template ([example](https://github.com/idescat/visual/blob/master/test/manual.html)).

If you are embedding more than one visualization in the page, LazyLoad will only include the javascripts needed for the first visualization in the **visual** function. Instead, include all the needed javascripts manually. Do not include LazyLoad. You will also need to specify an *id* and its size (in the *fixed* property: *[width, height]*) for each visualization.

# The Visual Object

**Visual** is executed by passing a Visual Object, or an array of Visual Objects, to the *visual* function.

```js
visual( {...} )
```

If you already have a defined *visual* function, you can run **Visual** like this:

```js
VisualJS.load( {...} )
```

The Visual Object accepts the following properties:

### General properties

#### lang
String ("ca", "es", "en"). Language. Default (*ca*) is set in [visual.setup.js](https://github.com/idescat/visual/blob/master/visual.setup.js).

#### title
String. Text of first level title.

#### footer
String. Text of footer. Since version 1, an array of strings is also accepted.

#### geo
String. Geographical area.

#### time
String (optional) or array of strings (required). Time period or periods.

Visual will treat the following string time formats using the *quarter* and *month* properties in [visual.setup.js](https://github.com/idescat/visual/blob/master/visual.setup.js): "20131" (first quarter of 2013) and "201301" (January 2013). Any other time pattern will be displayed untreated.

#### autoheading
Boolean. This determines whether the heading is built by composition from *title*, *geo* and *time*. If *false*, only *title* will be used as a heading. Default (*true*) is set in [visual.setup.js](https://github.com/idescat/visual/blob/master/visual.setup.js).

#### legend
Boolean. This determines whether the chart legend should be shown. Default (*true*) is set in [visual.setup.js](https://github.com/idescat/visual/blob/master/visual.setup.js).

The position of the legend in Cartesian charts is set in [visual.setup.js](https://github.com/idescat/visual/blob/master/visual.setup.js) (canvas *position* property). The position of the legend in maps is particular to each map and is set in the [canvas properties of the Visual map object](#canvas-properties-area-legend).

#### grid
Object with the following properties: *border* (number: grid border width), *line* (number: line width), *shadow* (number: line shadow width), *point* (number: point radius) and *markings* (array of markings). Default border (*0*), line (*2*), shadow (*4*) and point (*1*) are set in [visual.setup.js](https://github.com/idescat/visual/blob/master/visual.setup.js). A default collection of markings can also be set.

The *markings* property is supported when *type* is "tsline", "tsbar", "bar", "rank", "pyram" or "xy". See the [Flot API](https://github.com/flot/flot/blob/master/API.md#customizing-the-grid) for details.

Take into account that Visual comes with a default marking in the zero value of the Y-axis when *type* is "tsline". Specifying your own markings overrides it.

#### axis
Object with the following properties: *x* (boolean), *y* (boolean) and, since version 1, *ticks* (object) and *labels* (object). The first two determine whether the axes should be shown. Default (*true*) is set in [visual.setup.js](https://github.com/idescat/visual/blob/master/visual.setup.js).

When the axes are shown, *ticks* and *labels*, both objects with properties *x* (boolean) and *y* (boolean), can be used to hide (*false*) axis ticks and axis tick labels.

#### unit
Object with three properties: *label* (string or, since version 1, object), *symbol* (string or, since version 1, object) and symbol *position* ("start" or "end" string, or, since version 1, object). All properties are optional.

To support several value axes, since version 1, *label*, *object* and *symbol* can be objects with properties *x*, *y* and *z*. Currently, only in [XY charts](#xy-properties) *x* and *y* are used simultaneously.

Default (no label, no symbol, position: end) is set in [visual.setup.js](https://github.com/idescat/visual/blob/master/visual.setup.js).

Warning: *label* and *symbol* cannot contain HTML entities when *type* is "cmap".

####  dec
Number (non-negative integer) or, since version 1, object. Number of decimals in the data. This is used in tooltips, map legends and value axes. To support several value axes, since version 1, *dec* can be objects with properties *x*, *y* and *z*. Currently, only in [XY charts](#xy-properties) *x* and *y* are used simultaneously. When *dec* is a number, the same number of decimals is used in all value axes.

Although *dec* is an optional property, it is highly recommended that the number of decimals is specified: otherwise, all unneeded trailing zeros will be removed and computed values could be shown with more decimals than the original values. Default value can be set in [visual.setup.js](https://github.com/idescat/visual/blob/master/visual.setup.js).

#### type
String ("bar", "rank", "tsbar", "tsline", "pyram", "cmap" and, since version 1, "pie" and "xy"). Required. Chart type. This determines the *data* and *time* formats and the specific properties available.

#### data
Array. Required. This includes the data values but also series labels and identifiers. The format is determined by *type*.

#### id
String. In [simple mode](#simple-mode) and [manual mode](#manual-mode), this is the *id* of the HTML element where the visualization has to be embedded.

#### fixed
Array. In [simple mode](#simple-mode) and [manual mode](#manual-mode), this is the *[width, height]* in pixels of the visualization container.

#### callback
Function. This function will be called after the chart has been drawn. The *this* keyword will point to an object with the following properties: *id* (the chart's identifier: string), *chart* (a boolean indicating whether the chart is drawable or not: if *false*, [VisualJS.chart](#visualjschart) would not be defined), *heading* (the HTML of the heading) and *legend* (an object with legend information).

The *legend* object is *null* unless the chart type is "cmap", [data](#data-6) includes a *val* property and [grouped.color](#grouped) is not specified. In this case, colors are automatically assigned to map regions.

When *legend* is not *null*, it has three properties (three arrays): *color*, *text* and *symbol*. When [grouped](#grouped) is not specified, the first element in each array exposes information about the lighter color and the second element about the darker one. Since version 1, when [grouped](#grouped) is specified but not *grouped.color*, the elements of *color*, *text* and *symbol* expose information about each group.

The elements of the *color* array are objects with three properties (numbers): *r*, *g* and *b* (RGB color). The elements of the *text* array are strings (value plus unit information the lighter/darker color has been assigned to). The elements of the *symbol* array are three possible strings: "<=", "==" or ">=" (the comparison operators associated with the *text* elements). See example [adv-05.html](https://github.com/idescat/visual/blob/master/test/adv-05.html).

The *callback* property will be ignored if it is included in a Visual Object passed to [VisualJS.iframe](#visualjsiframe).

Examples:  [adv-01.html](https://github.com/idescat/visual/blob/master/test/adv-01.html), [adv-02.html](https://github.com/idescat/visual/blob/master/test/adv-02.html), [adv-03.html](https://github.com/idescat/visual/blob/master/test/adv-03.html), [adv-05.html](https://github.com/idescat/visual/blob/master/test/adv-05.html).

#### show
Boolean. This determines whether the chart should be shown. When *false*, all the necessary files will be included but the chart will not be inserted: you will need to use a callback function that executes [VisualJS.chart](#visualjschart) at some point. Default is *true*.

```js
VisualJS.load({
	show: false,
	callback: function(){
		if(this.chart && window.confirm("Are you sure you want to see this chart?")){
			VisualJS.chart();
		}
	},
	...
});
```
#### listen
Boolean. When *true*, Visual will register a listener for events of type "message".  Default is *false*.

Currently, Visual will only respond to messages that include a string that represents a JSON object with two properties: *action* and *id*. The only *action* currently supported is "send". The *id* must specify the [id](#id) of an existing [Visual Object](#the-visual-object). If *id* is not specified, the last existing *id* will be used.

```js
{ "action": "send", "id": "visual" }
```

The listener will post a message back to the source with a string representation of the requested Visual Object after validation.

On error, the Visual Object will only contain the *type* property (value: "error") and the *data* property (array of size 1 because simultaneous error messages are not currently supported). The first element in *data* will be an object with only two properties: *id* (error type) and *label* (text in English). Error type "400" is used when *action* has not been specified or has an incorrect value.  Error type "404" is used when *id* does not identify an existing Visual Object.

### *bar* properties

Distribution of a categorical variable (vertical bar chart).

```js
visual({
	lang: "en", //Default language is Catalan ("ca"). Set in visual.setup.js
	title: "BAR example",
	time: "2012",
	footer: "The source goes here.",
	unit: {label: "people"},
	dec: 0,
	type: "bar",
	data : [["0-14", 7329], ["15-64", 30231], ["65-84", 6485], ["85+", 1254]]
	/* Same as:
		by: [ "0-14", "15-64", "65-84", "85+" ],
		data: [ 7329, 30231, 6485, 1254 ]
	*/
});
```

Examples: [bar.html](https://github.com/idescat/visual/blob/master/test/bar.html),  [bar2.html](https://github.com/idescat/visual/blob/master/test/bar2.html), [barg.html](https://github.com/idescat/visual/blob/master/test/barg.html)

#### time
String. Time period.

#### data
Array of numbers, array of arrays or, since version 1, array of objects. Required.

If *by* has not been specified, an array of arrays is expected with as many elements as categories. Each element is an array with two elements: a string (label) and a number (value).

If *by* is specified, then *data* can be an array of numbers (values) or, since version 1, an array of objects (like in *[pyram](#pyram-properties)*), each with two properties: *label* (string) and *val* (array of numbers).

#### by
Array of strings. It contains the value labels. See the *data* property.

#### range
Array of numbers (minimum, maximum). The first element must be lower than the second one. This array sets the range of the y-axis.

Since version 1, *null* can also be used instead of the minimum or the maximum. If the minimum is *null*, it will be set to the lowest negative value, or zero if all values are non-negative. If the maximum is *null*, it will be set to the highest value.

Since version 1, *range* can also be specified as an object with a property *y* (array of numbers).

### *rank* properties

Ranking (horizontal bar chart).

```js
visual({
	lang: "en", //Default language is Catalan ("ca"). Set in visual.setup.js
	title: "RANK example (40 data)",
	geo: "Catalonia",
	time: "2009",
	unit: {symbol: "K"},
	dec: 0,
	axis:{
		ticks:{
			y:false,
			x:true
		}
	},
	type: "rank",
	data: [
		["Val d'Aran", 20300],
		["Pallars Jussà", 19300],
		["Ripollès", 19100],
		["Urgell", 18900],
		["Conca de Barberà", 18800],
		["Gironès", 18700],
		["Pallars Sobirà", 18700],
		["Alta Ribagorça", 18600],
		["Cerdanya", 18600],
		["Garrotxa", 18600],
		["Pla de l'Estany", 18600],
		["Barcelonès", 18300],
		["Priorat", 18300],
		["Ribera d'Ebre", 18200],
		["Segrià ", 18100],
		["Garrigues", 18000],
		["Baix Empordà", 17700],
		["Maresme", 17700],
		["Alt Camp", 17600],
		["Noguera", 17600],
		["Tarragonès", 17600],
		["Terra Alta", 17600],
		["Segarra", 17400],
		["Alt Empordà", 17300],
		["Baix Penedès", 17300],
		["Solsonès", 17300],
		["Vallès Occidental", 17300],
		["Berguedà", 17200],
		["Baix Camp", 17100],
		["Pla d'Urgell", 17100],
		["Montsià", 17000],
		["Alt Penedès", 16900],
		["Bages", 16900],
		["Baix Ebre", 16900],
		["Garraf", 16900],
		["Alt Urgell", 16600],
		["Selva", 16300],
		["Osona", 16200],
		["Vallès Oriental", 16200],
		["Baix Llobregat", 16000],
		["Anoia", 15800]
	]
});
```

Examples:  [rank.html](https://github.com/idescat/visual/blob/master/test/rank.html), [rank10.html](https://github.com/idescat/visual/blob/master/test/rank10.html)

#### time
String. Time period.

####  data
Array of arrays or, since version 1, array of numbers. Required.

If *by* has not been specified, an array of arrays is expected with as many elements as categories. Each element is an array with two elements: a string (label) and a number (value).

If *by* is specified, then *data* must be an array of numbers (values).

#### by
Array of strings. It contains the value labels. See the *data* property. Added in version 1.

#### range
Number (multiplier) or array of numbers (minimum, maximum). By default, the multiplier is 1.02 (increase the x-axis by 2%). An array (where the first element must be lower than the second one) can also be used to set the range of the x-axis.

Since version 1, *null* can also be used instead of the minimum or the maximum. If the minimum is *null*, it will be set to the lowest negative value, or zero if all values are non-negative. If the maximum is *null*, it will be set to the highest value.

Since version 1, *range* can also be specified as an object with a property *x* (array of numbers).

### *pie* properties

Distribution of a categorical variable in a circle chart (available since version 1).

```js
visual({
	lang: "en", //Default language is Catalan ("ca"). Set in visual.setup.js
	title: "PIE example",
	dec: 0,
	type: "pie",
	data : [ ["Men", 7329], ["Women", 30231] ]
	/* Same as:
		by: [ "Men", "Woman" ],
		data: [ 7329, 30231 ]
	*/
});
```

Examples: [pie.html](https://github.com/idescat/visual/blob/master/test/pie.html)

#### time
String. Time period.

#### data
Array of numbers or array of arrays. Required.

If *by* has not been specified, an array of arrays is expected with as many elements as categories. Each element is an array with two elements: a string (label) and a number (value).

If *by* is specified, then *data* must be an array of numbers (values).

#### by
Array of strings. It contains the value labels. See the *data* property.

### *tsbar* properties

Stacked/non-stacked time series (vertical bar chart).

```js
visual({
	lang: "en", //Default language is Catalan ("ca"). Set in visual.setup.js
	title: "TSBAR example",
	geo: "Catalonia",
	time : [
		"1998", "1999", "2000", "2001", "2002",
		"2003", "2004", "2005", "2006", "2007",
		"2008", "2009", "2010", "2011", "2012"
	],
	unit: {label: "M", symbol: "€", position: "start"},
	dec: 1,
	type: "tsbar",
	 data: [
		{
			 label: "Exports",
			 val: [
					27147.8, 27890.6, 33796.5, 36694.5, 37275.9,
					37648.5, 39485.1, 42703.4, 47218.8, 49679.8,
					50515.7, 41461.7, 48871.6, 54999.9, 58321.7
			 ]
		},
		{
			 label: "Imports",
			 val: [
					36203.7, 40316.5, 48761.7, 50497.9, 51891.8,
					54344.7, 60731, 67813.3, 74787.8, 80363.4,
					77233.9, 57663.8, 67621.1, 72280.2, 69343.1
			 ]
		}
	 ]
});
```

Examples:  [tsbar.html](https://github.com/idescat/visual/blob/master/test/tsbar.html), [tsbar2.html](https://github.com/idescat/visual/blob/master/test/tsbar2.html), [tsbarns.html](https://github.com/idescat/visual/blob/master/test/tsbarns.html)

#### time
Array of strings. Required. Time periods.

#### data
Array of objects. Required. The array contains as many elements as series. Each element is an object with two properties: *label* (string) and *val* (array of numbers).

#### stacked

Boolean. Default: *false*. When bars are not stacked, only three series are allowed.

#### range
Array of numbers (minimum, maximum). The first element must be lower than the second one. This array sets the range of the y-axis.

Since version 1, *null* can also be used instead of the minimum or the maximum. If the minimum is *null*, it will be set to the lowest negative value, or zero if all values are non-negative. If the maximum is *null*, it will be set to the highest value.

Since version 1, *range* can also be specified as an object with a property *y* (array of numbers).

### *tsline* properties

Time series (line chart).

```js
visual({
	lang: "en", //Default language is Catalan ("ca"). Set in visual.setup.js
	title: "TSLINE example",
	geo: "Catalonia",
	time : [
		"1998", "1999", "2000", "2001", "2002",
		"2003", "2004", "2005", "2006", "2007",
		"2008", "2009", "2010", "2011", "2012"
	],
	unit: {label: "M", symbol: "€", position: "start"},
	dec: 1,
	range: [null, null],
	type: "tsline",
	data: [
		{
				 label: "Exports",
				 val: [
						27147.8, 27890.6, 33796.5, 36694.5, 37275.9,
						37648.5, 39485.1, 42703.4, 47218.8, 49679.8,
						50515.7, 41461.7, 48871.6, 54999.9, 58321.7
				 ]
			},
			{
				 label: "Imports",
				 val: [
						36203.7, 40316.5, 48761.7, 50497.9, 51891.8,
						54344.7, 60731, 67813.3, 74787.8, 80363.4,
						77233.9, 57663.8, 67621.1, 72280.2, 69343.1
				 ]
			}
	 ]
});
```

Examples:  [tsline.html](https://github.com/idescat/visual/blob/master/test/tsline.html), [tsline2.html](https://github.com/idescat/visual/blob/master/test/tsline2.html)

#### time
Array of strings. Required. Time periods.

#### data
Array of objects. Required. The array contains as many elements as series. Each element is an object with two properties: *label* (string) and *val* (array of numbers).

#### range
Array of numbers (minimum, maximum). The first element must be lower than the second one. This array sets the range of the y-axis.

Since version 1, *null* can also be used instead of the minimum or the maximum. If the minimum is *null*, it will be set to the lowest negative value, or zero if all values are non-negative. If the maximum is *null*, it will be set to the highest value.

If *range* is not specified (or is *null*), it will be set to *[minimum value, maximum value]*. If there are neither zeros nor negative values in the data, this is different from specifying *[null, null]* (in such case, *[null, null]* is equivalent to *[0, null]*).

Since version 1, *range* can also be specified as an object with a property *y* (array of numbers).


### *pyram* properties

Population pyramid.

```js
visual({
	lang: "en", //Default language is Catalan ("ca"). Set in visual.setup.js
	title: "PYRAM example",
	geo: "A country",
	time: "2012",
	unit: {label: "people"},
	dec: 0,
	type: "pyram",
	by: [
			"0-4", "5-9", "10-14", "15-19", "20-24", "25-29",
			"30-34", "35-39", "40-44", "45-49", "50-54", "55-59",
			"60-64", "65-69", "70-74", "75-79", "80-84", "85-89",
			"90-94", "95-99", "100+"
	],
	data: [
			{
				 label: "Men",
				 val: [
						130229, 132460, 109072, 115983, 133972, 166757,
						207016, 211782, 195472, 176832, 152151, 122107,
						117375, 99405, 80274, 73283, 47597, 24195, 6997, 1532, 260
				 ]
			},
			{
				 label: "Women",
				 val: [
						124757, 112944, 103163, 104773, 122879, 152743,
						196767, 193411, 194849, 174780, 155177, 133712,
						126386, 117169, 98444, 99468, 76448, 47515,17929, 4284, 548
				 ]
			}
	]
});
```

Examples:  [pyram.html](https://github.com/idescat/visual/blob/master/test/pyram.html)

#### time
String. Time period.

#### data
Array of objects. Required. The array contains two elements: one for each sex. Each element is an object with two properties: *label* (string) and *val* (array of numbers).

#### by
Array of strings. Required. Each element is an age label.

#### range
Number (multiplier) or array of numbers (minimum, maximum). By default, the multiplier is 1.02 (increase the x-axis by 2%). An array (where the first element must be lower than the second one and it will be ignored if it is different than zero or null) can also be used to set the range of the x-axis.

Since version 1, *null* can also be used instead of the maximum to set it to the highest value.

Since version 1, *range* can also be specified as an object with a property *x* (array of numbers).


### *xy* properties

Scatter plot. Added in version 1.

```js
visual({
	lang: "en", //Default language is Catalan ("ca"). Set in visual.setup.js
	title: "XY example",
	unit: {
		label: {x: "M", y: "M"},
		symbol: {x: "$", y: "$"},
		position: {x: "start", y: "start"}
	},
	dec: 0,
	type: "xy",
	data: [
		{
			by: ["1998", "1999", "2000", "2001", "2002", "2003", "2004", "2005", "2006", "2007", "2008", "2009", "2010", "2011", "2012", "2013", "2014", "2015", "2016"],
			x: {
				label: "Imports",
				val: [78349.40,76988.20,72908.70,67859.80,70323.90,72173.20,67621.10,57663.80,77233.90,80337.70,74787.80,67813.30,60731.00,54344.70,51891.80,50497.90,48761.70,40316.50,36203.70]
			},
			y: {
				label: "Exports",
				val: [65160.60,63906.30,60313.70,58981.30,58880.70,54989.20,48871.60,41461.70,50515.70,49678.40,47216.00,42703.40,39485.10,37648.50,37275.90,36694.50,33796.50,27890.60,27147.80]
			}
			/*Same as
			x: "Imports",
			y: "Exports",
			val: [ [78349.40,65160.60], [76988.20,63906.30], ... ]
			*/
		}
	]
});
```

Examples:  [xy.html](https://github.com/idescat/visual/blob/master/test/xy.html)

#### time
String. Time period.

#### data
Array of objects. Required. The array contains as many elements as data sets. Each element is an object with two required properties: *x* and *y*. It can also include a *label* property (string) to provide a text to attach to the data set and a *by* property (array of strings) to provide a text to attach to each data point.

*x* and *y* are used to provide the axes information. They support two different formats.

They can be objects that have a *label* property (string) to describe the axis and a *val* property (array of numbers) with the axis values.

Or they can be strings that contain the axis label. In this format, an additional *val* property (array of arrays) is required to provide the data points ([x,y]).

#### range

Object with two properties (one per axis): *x* and *y*. Both of them are an array of numbers (minimum, maximum). The first element in the array must be lower than the second one.

*null* can also be used instead of the minimum or the maximum. If the minimum is *null*, it will be set to the lowest negative value, or zero if all values are non-negative. If the maximum is *null*, it will be set to the highest value.


### *cmap* properties

Choropleth map.

```js
visual({
	lang: "en", //Default language is Catalan ("ca"). Set in visual.setup.js
	title: "CMAP example",
	geo: "Catalonia",
	time: "2001",
	unit: {symbol: "%"},
	dec: 2,
	by: "com",
	type: "cmap",
	data: [
			{id: "01", val: 85.50},
			{id: "02", val: 79.40},
			{id: "03", val: 80.91},
			{id: "04", val: 86.50},
			{id: "05", val: 83.01},
			{id: "06", val: 79.04},
			{id: "07", val: 82.74},
			{id: "08", val: 77.31},
			{id: "09", val: 86.48},
			{id: "10", val: 79.94},
			{id: "11", val: 65.79},
			{id: "12", val: 73.04},
			{id: "13", val: 70.35},
			{id: "14", val: 89.96},
			{id: "15", val: 84.79},
			{id: "16", val: 91.06},
			{id: "17", val: 75.31},
			{id: "18", val: 92.95},
			{id: "19", val: 89.95},
			{id: "20", val: 82.50},
			{id: "21", val: 77.03},
			{id: "22", val: 86.48},
			{id: "23", val: 90.73},
			{id: "24", val: 86.06},
			{id: "25", val: 88.94},
			{id: "26", val: 91.67},
			{id: "27", val: 88.38},
			{id: "28", val: 88.68},
			{id: "29", val: 92.49},
			{id: "30", val: 90.66},
			{id: "31", val: 88.24},
			{id: "32", val: 86.17},
			{id: "33", val: 83.71},
			{id: "34", val: 77.71},
			{id: "35", val: 90.53},
			{id: "36", val: 74.20},
			{id: "34", val: 91.87},
			{id: "38", val: 88.66},
			{id: "39", val: 77.98},
			{id: "40", val: 71.31},
			{id: "41", val: 75.56}
	]
});
```

Examples:  [cmap.html](https://github.com/idescat/visual/blob/master/test/cmap.html), [cmap-prov08.html](https://github.com/idescat/visual/blob/master/test/cmap-prov08.html), [cmap-com.html](https://github.com/idescat/visual/blob/master/test/cmap-com.html), [cmap-f0.html](https://github.com/idescat/visual/blob/master/test/cmap-f0.html), [cmap-f020.html](https://github.com/idescat/visual/blob/master/test/cmap-f020.html), [cmap-groups1.html](https://github.com/idescat/visual/blob/master/test/cmap-groups1.html), [cmap-groups2.html](https://github.com/idescat/visual/blob/master/test/cmap-groups2.html),
[cmap-groups3.html](https://github.com/idescat/visual/blob/master/test/cmap-groups3.html), [cmap-high1.html](https://github.com/idescat/visual/blob/master/test/cmap-hight1.html), [cmap-high2.html](https://github.com/idescat/visual/blob/master/test/cmap-hight2.html),
[cmap-usa.html](https://github.com/idescat/visual/blob/master/test/cmap-usa.html), [cmap-ny.html](https://github.com/idescat/visual/blob/master/test/cmap-ny.html), [cmap-spainnuts2.html](https://github.com/idescat/visual/blob/master/test/cmap-spainnuts2.html), [cmap-spainnuts3.html](https://github.com/idescat/visual/blob/master/test/cmap-spainnuts3.html), [cmap-norway.html](https://github.com/idescat/visual/blob/master/test/cmap-norway.html), [cmap-eu28.html](https://github.com/idescat/visual/blob/master/test/cmap-eu28.html)

#### time
String. Time period.

####  data
Array of objects. Required. The array contains as many elements as map regions. Each element is an object with at least one property: the region *id* (string). In this case, a map  will be created with the included regions highlighted. If *val* (number) is included, it will be used to automatically assign colors to regions, unless [grouped](#grouped) has been specified. In that case, the *group* property (counter starting with 1) is required and will be used to assign colors, but *val* can still be specified if needed. If *label* (string) is included in all objects, it will be used to name the map regions; otherwise, the [label in the map](#id-properties-id-label) will be used (Visual will only check the first object to decide whether the *label* property has been provided or not).

####  grouped
Object with at least one property: *label* (array of strings). Each element in this array is a group label string (the first label will be attached to regions with a *group* property of 1 in **data**, and so on). A second property (*color*, array of strings) can be provided to assign a custom color to each group. Colors must be specified as three two-digit hexadecimal numbers, starting with a # sign (for example, "#000000" means black).

Since version 1, when property *val* is present in **data** and *grouped.color* is not set, the colors will be assigned assuming that the *grouped* information has been provided taking into account *val* (in ascending or descending order).

####  by
String. Required. Selects a certain map. Possible values ("mun", "com", "prov", etc.) are set in [visual.setup.js](https://github.com/idescat/visual/blob/master/visual.setup.js).

####  range
Number or array. This determines the color assignment. When it is a number, it must be between 0 and 0.49. Default: 0.05, which means color assignment excludes values below the 5th percentile and above the 95th percentile. When it is an array, it defines a range: it has two and only two elements. The first one (number) is a minimum and the second one (number) is a maximum. Colors will be assigned between those values.

# Maps
The following sample maps are provided:

* European Union by countries (1 map)
* United States of America by states (1 map)
* Norway by municipalities (1 map)
* Spain by NUTS 2 and 3 (2 maps)
* Catalonia by provinces, regions of the Territorial Plan, counties and municipalities (5 maps)
* Provinces of Catalonia by municipalities (4 maps)
* Counties of Catalonia by municipalities (45 maps)

These are stored in the [maps folder](https://github.com/idescat/visual/tree/master/maps). The [map maker](https://github.com/idescat/visual/tree/master/maps/maker/) allows you to preview these maps and fine-tune them.

A map is a UTF-8 Javascript file that adds a new property (the name of the map) to the VisualJS.map object. The value of this new property is a Visual map object.

### Visual map object

#### GeoJSON properties: features

Geographic information must be provided in the [GeoJSON](http://geojson.org) format: it must be a feature collection object (a GeoJSON object with the type "FeatureCollection"). Simply copy the *features* property of the GeoJSON object into the Visual map object.

#### Projection properties: projection, scale, center

*projection* (string) must be a valid [D3 geo projection](https://github.com/mbostock/d3/wiki/Geo-Projections) function name. *scale* is the projection scale (a number) and *center* is the projection center (a coordinate array).

	projection: "mercator",
	scale: 9000,
	center: [1.74, 41.7],

If a projection does not support centering (for example, Albers USA), *center* is optional and, if present, will be ignored.

Visual does not currently support rotation.

#### Identification properties: id, label

Use *id* and *label* to specify the properties in *features* that contain the regions' identifiers and the regions' labels.

	id: "STATE",
	label: "NAME",

#### Canvas properties: area

Use *area* to provide the size in pixels (width, height) of the canvas where your projection will be drawn.

	area: [500, 500],

These values will not determine the final size of your map (maps will scale to the available space): they are only important for determining the *scale* and *center* values.

(Before version 1, there was also a *legend* property required to specify the location in pixels of the map legend in the canvas. In version 1, the layout of the map legend was changed. As a result, since version 1, the legend is always located below the map and the *legend* property is no longer necessary.)

### Map setup

Maps must be declared in [visual.setup.js](https://github.com/idescat/visual/blob/master/visual.setup.js). To include a new map, edit [visual.setup.js](https://github.com/idescat/visual/blob/master/visual.setup.js) and create a new property with the name of the map inside VisualJS.setup.map. This name must match the name in VisualJS.map (in the Javascript map file) and will be used in the [by](#by-1) property. The value of this property must be an object with two properties: the address of the map (*js*) and an existence function (*exists*).

Once your map has been added to [visual.setup.js](https://github.com/idescat/visual/blob/master/visual.setup.js), use the [map maker](https://github.com/idescat/visual/tree/master/maps/maker/) to fine-tune it.

# Public functions

## VisualJS.load

This is the main function. It loads the data and, if the property **show** is *true* (default), draws the chart using [VisualJS.chart](#visualjschart). It only accepts one argument: a [Visual Object](#the-visual-object) or an array of Visual Objects.

```js
VisualJS.load( {...} ); //Same as: visual( {...} );
```

This function is used in [webpage mode](#webpage-mode).

## VisualJS.chart

This function does the actual drawing of the chart. It does not accept any argument.

```js
VisualJS.chart( );
```

It will usually be invoked inside a [callback function](#callback).

Examples:  [adv-01.html](https://github.com/idescat/visual/blob/master/test/adv-01.html), [adv-03.html](https://github.com/idescat/visual/blob/master/test/adv-03.html).

## VisualJS.iframe

In [simple mode](simple-mode), this function is used to embed visualizations. It accepts two arguments: a [Visual Object](#the-visual-object) and a string (a CSS file address or CSS rules). If the Visual Object contains a [callback](#callback) property, it will be ignored.

```js
VisualJS.iframe( {...} , "http://mydomain/path/iframe.css" );
```

Example: [simple.html](https://github.com/idescat/visual/blob/master/test/simple.html).

## VisualJS.compare

This function creates a comparison visualization (two charts side by side). It accepts one argument: an object with the following properties:

### title
String. Text of title.

### footer
String. Text of footer.

### load
Array of two [Visual Objects](#the-visual-object) (required).

### css
String or array of two strings. The strings can be CSS file addresses or CSS rules. When two strings are provided, the first style is used in the left chart and the second one is used in the right chart.

Example:  [adv-04.html](https://github.com/idescat/visual/blob/master/test/adv-04.html).

# Dependencies

**Visual** uses the following libraries internally:

* [LazyLoad](https://github.com/rgrove/lazyload/)
* [D3](https://d3js.org)
* [jQuery](https://jquery.com/), required by Flot
* [Flot](http://www.flotcharts.org) ([stack](https://github.com/flot/flot/blob/master/jquery.flot.stack.js), [categories](https://github.com/flot/flot/blob/master/jquery.flot.categories.js), [pie](https://github.com/flot/flot/blob/master/jquery.flot.pie.js)), [Flot orderBars](http://en.benjaminbuffet.com/labs/flot/), [Flot Pyramid](https://github.com/asis/flot-pyramid), [Flot axisLabels](http://github.com/markrcote/flot-axislabels)
* [ExplorerCanvas](https://code.google.com/p/explorercanvas/)

These libraries are only loaded when needed.

For convenience, they are included in the [lib folder](https://github.com/idescat/visual/tree/master/lib) but you can use any location (for example, a CDN) in [visual.setup.js](https://github.com/idescat/visual/blob/master/visual.setup.js).

# Known limitations

[D3](https://d3js.org) requires a modern browser (versions of Internet Explorer prior to 9 are not supported). **Visual** uses D3 only for choropleth maps (chart type: "cmap").

The non-stacked time series bar chart supports a maximum of three series. This is not a technical limitation but a visual one.

# How to contribute

You are welcome to contribute to this project! Areas where your participation can be very useful are, for example:

* Support for new **chart types**
* **Maps** of your territory

To contribute, [fork this repository](https://github.com/idescat/visual/fork), push changes to your personal fork and send a pull request.
