# LeafletSlider-tutorial
This is a tutorial repo, written by [Brian Geyer](https://github.com/geyerbri), to introduce people to using the [Leaflet Time-Slider plugin](https://github.com/geyerbri/LeafletSlider) in a Leaflet map. Fork this repo and then work through this tutorial by writing into the blank index.html file. Once completed, your map will look like [this example map](https://chi-initiative.github.io/LeafletSlider-tutorial/complete.html).

NOTE: This tutorial links to my fork of the Leaflet Time-Slider plugin because, as of October 2020, neither the [original repo](https://github.com/dwilhelm89/LeafletSlider) nor the [Falke-Design-managed fork](https://github.com/Falke-Design/LeafletSlider) are being actively updated with fixes and improvements.

---
## Table of Contents

* [Set up html file for map](#set-up-html-file-for-map)
* [Add resources for the plugin](#add-resources-for-the-plugin)
* [Add popups](#add-popups)
* [Enable the Slider](#enable-the-slider)
* [Control multiple markers simultaneously in multiple groups](#control-multiple-markers-simultaneously-in-multiple-groups)

<sub><a href="http://ecotrust-canada.github.io/markdown-toc/">*Table of contents generated with markdown-toc*</a></sub>

---

## Set up html file for map
Begin by setting up your html file with the basic structure.

```js
<html lang="en">
    <head>

    </head>
    <body>

    </body>
</html>
```

Fill the `<head>` with the stylesheets and scripts required for Leaflet.

```html
<!--jquery js-->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
<!-- leaflet files -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.7.1/leaflet.css" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.7.1/leaflet.js"></script>
```

Write some CSS into the `<head>` to style the map you'll be creating. These settings will create a map, centered in the window.

```html
<style>
    #myMap {
        width: 800px;
        max-width: 100%;
        height: 450px;
        margin: 0;
        position: absolute;
        top: 50%;
        left: 50%;
        -ms-transform: translate(-50%, -50%);
        transform: translate(-50%, -50%)
    }
</style>
```

If you'd prefer a map that takes up the entire window, you can use this instead:

```html
<style>
    body{
        margin: 0
    }
    #myMap {
        width: 100%;
        height: 100%;
    }
</style>
```

In the `<body>`, add a div container, into which you'll be populating the map.

```html
<div id="myMap">If you can read this, either an error has occurred, or you're getting cute with the developer tools of your browser.</div>
```

Still inside `<body>`, create a script block, for creating the map. Set its view however you'd like; or just use the one I've defaulted to. Load a tile layer with one of your choice; for instance, this example uses one from [OpenStreetMap.org](https://www.openstreetmap.org/). It's also always a good idea to include the attribution for where you got your tile layer (Leaflet automatically provides its own attribution link, as long as you haven't purposely disabled it).

```html
<script >
    // Create map (map1) inside div with #myMap ID; set the view
    var map1 = L.map('myMap').setView([46.26,-119.15], 12);

    // Add tileLayer to map1
    L.tileLayer('https://{s}.tile.openstreetmap.fr/hot/{z}/{x}/{y}.png', {
        attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
    }).addTo(map1);
</script>
```

> Note: OpenStreetMap provides tilesets for small projects and very light use; their [Tile Usage Policy](https://operations.osmfoundation.org/policies/tiles/) calls this "marginal" and specifically forbids "heavy use." The linked usage policy includes resources for finding a suitable tileset for heavy use cases, or for creating one's own server to host the OpenStreetMaps data and use it for such cases.

After adding this code to your file, you can go to your repo's Settings tab, scroll down to the GitHub Pages section, choose "master" from the Sources dropdown list, and click "Save." Keep refreshing the page until this same section has a green bar with the text, "Your site is published at..." You can click on this link to view your page, with the map loaded. If you go to this link and the only thing that loads is the text, "If you can read this, either an error has occurred, or you're getting cute with the developer tools of your browser", then go back through these first few steps to confirm you have closely followed them.

If the page loads with a map, then your HTML file is properly set up.

## Add resources for the plugin
The Leaflet Time-Slider plugin is meant to control the display of markers, one at a time, via sliding control. To accomplish this requires adding additional JS scripts and a stylesheet to your `<head>` section. The first set of these, the jquery UI script and stylesheet, is available via jquery's CDN:

```html
<!--jQuery UI files-->
<link rel="stylesheet" href="https://code.jquery.com/ui/1.12.1/themes/base/jquery-ui.css" />
<script src="https://code.jquery.com/ui/1.12.1/jquery-ui.js" integrity="sha256-T0Vest3yCU7pafRw9r+settMBX6JkKN06dqBnpQ8d30=" crossorigin="anonymous"></script>
<!--jQuery UI Touch js, required for touchscreen use-->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jqueryui-touch-punch/0.2.3/jquery.ui.touch-punch.min.js"></script>
```

You'll also need to add the plugin's locally-saved JS script, which is inside the "js" folder.

```html
<!--Leaflet Time-Slider plugin js;
find maintained version at https://github.com/geyerbri/LeafletSlider-->
<script src="js/SliderControl.js"></script>
```

## Add popups
Adding a few popups directly within the html file will serve as a basic example of how this plugin works.

Begin with a list of a few markers, added as a set of geoJSON-styled data. As a reminder, geoJSON files put the longitude first and latitude second as the coordinates, so these are reversed from how they are usually read.

Add the following into the `<script>` section, where you previously put the map and tile layer:

```js
var dataset1 = [
    {
        type: 'Feature',
        geometry: {
            type: 'Point',
            coordinates: [-119.0977191, 46.2465524]
        },
        properties: {
            title: 'Popup 1',
            description: 'Hello World!',
            time: '1992/01'
        }
    },
    {
        type: 'Feature',
        geometry: {
            type: 'Point',
            coordinates: [-119.201977, -46.257038]
        },
        properties: {
            title: 'Popup 2',
            description: 'Hello again, World!',
            time: '1992/07'
        }
    },
    {
        type: 'Feature',
        geometry: {
            type: 'Point',
            coordinates: [-119.1080419, 46.2309332]
        },
        properties: {
            title: 'Popup 3',
            description: 'Hello once again, World!',
            time: '1992/06'
        }
    }
];
```

After copying in this geoJSON-formatted data, you'll need create a new variable `optionsObject`, which will define via a function the popup structure that will be attached to each marker (via a second variable, explained below). This code will reference different parts of the "properties" spelled out in the data. It needs to be added *below* the geoJSON data, map, and tile layer portions of the code:

```js
var optionsObject = {
    onEachFeature: onEachFeature
};
function onEachFeature (feature, layer) {
    var content = "<div style='clear: both'></div><div><h4>" + feature.properties.title + "</h4><p>" + feature.properties.time + "</p><p>" + feature.properties.description + "</p></div>";
    layer.bindPopup(content, {closeButton: true});
};
```

The second variable, `content`, describes which parts of the geoJSON data fits into which part of the popup that will be generated. Each popup is bound to each marker, added to the existing map as a new layer. When binding the popup, it also adds a close-popup button in its default location (the top-right of the popup).

Under where this has all been specified, add this new variable, which defines a group of geoJSON-defined markers and the associated popups, based upon the data.

```js
var group1 = L.geoJSON(dataset1, optionsObject);
```

To view these markers and popups, add this line, once again below the previous line:

```js
group1.addTo(map1);
```

Once GitHub updates with this most recent change, you can load your repo's website and see these markers on your map. Click on each of them to see their unique popups.

## Enable the Slider

To enable the slider, first delete the last line added:

<code><s>group1.addTo(map1);</s></code>

Instead, define the slider variable as the slider function. This example uses a number of options changed from their defaults.

```js
var sliderControl1 = L.control.sliderControl( {
    layer: group1, // REQUIRED
    alwaysShowDate: true, // to make the date box below the slider persistent
    showAllPopups: false, // to show all popups, instead of only one; same as the popup option "autoClose: false"
    showPopups: false // to set it so, when a marker is generated by the slider, its popup doesn't automatically display
});
```

The first option here, `layer` is required any time the `sliderControl` function is used; it has been defined as the only group created so far. This setting turns each item in the group into its own layer. (See the tutorial below for how to set this option when controlling multiple groups in different layers, so that each group acts as its own layer.)

The option `alwaysShowDate` controls whether or not the date for the current setting is displayed below the slider bar. The `showAllPopups` option controls whether or not the current popup closes when another popup is opened; when set to "false", only one popup will be open at one time. And finally, `showPopups` controls whether or not the popup for any given marker is automatically open when the slider generates that marker. When set to "false", you must click on the popup to view its content.

You can view the other available options in the [plugin's GitHub repo README](https://github.com/geyerbri/LeafletSlider#options).

Lastly, once you've configured `sliderControl` however you wish, add the slider to the map and then initialize it.

```js
// Add the slider to the map
map1.addControl(sliderControl1);

// Initialize the slider
sliderControl1.startSlider();
```

After adding these two functions, and your repo has updated online, you can go to your repo's webpage to see the slider in action. If you noticed before, the `time` values in each geoJSON entry are not in chronological order – the second and third entries are flipped, chronologically. This is to show that this plugin automatically reorders entries by default.

## Control multiple markers simultaneously in multiple groups
The way the Time-Slider plugin works, it creates a new layer for each marker in the geoJSON layer, then uses the slider to control the display of those individual layers. This means it is also possible to create a new `groupLayer` – which is its own specific thing for Leaflet – that contains each geoJSON collection of markers. Then, the plugin turns each collection into a controllable individual layer, rather than each marker separately.

Doing this requires a few additional steps in the `<script>`, but this tutorial will go even further to show how to create a geoJSON layer by importing data from an external geoJSON file. Then it'll show how to take the two geoJSON layers and put them into a single `groupLayer`, which the plugin can then control. 

This tutorial includes a sample `.json` file, inside of the "data" folder. Start by copying the middle two lines into your code, in between the lines shown:

```js
var group1 = L.geoJSON(dataset1, optionsObject);

$.getJSON("data/popups.json", function(json) {
    var group2 = L.geoJSON(json, optionsObject);

var sliderControl1 = L.control.sliderControl( {;
```

Leaflet comes with the ability to import external geoJSON data; to use other forms of data, consider looking at the [leaflet-omnivore plugin](https://github.com/mapbox/leaflet-omnivore). The first line added uses the built-in capability to grab the geoJSON data. The second creates the second geoJSON layer and populates it with the external data, by using the same popup-writing function used before. If your geoJSON data has a different structure, you can write a different function for going over that different structure and write the popups.

(To instead add more geoJSON data as a second dataset within `<script>` and not via an external file, do not use the above code. Instead, create `dataset2` the same way as you did `dataset1`, but with different data. And then create `group2` the same way you did `group1`, but with `dataset2` for its contents instead of `dataset1`.)

After adding those lines, add the following middle three, again in between the top and bottom lines shown:

```js
    var group2 = L.geoJSON(json, optionsObject);

    group1.options.time = "1992";
    group2.options.time = "1993";
    var multiLayers = L.layerGroup([group1, group2]);

var sliderControl1 = L.control.sliderControl( {;
```

The first two lines set the "time" option for each geoJSON layer, so that Time-Slider has the required "time" attribute to work upon. The final line creates the `layerGroup` with both geoJSON layers; to add more geoJSON layers, first create them via one of the two methods above, set their "time" option, then add them to this list.

Next, change the `layer` value inside `sliderControl1` from `group1` to `multiLayers`:

```js
var sliderControl1 = L.control.sliderControl( {
    layer: multiLayers, // REQUIRED
    alwaysShowDate: true, // to make the date box below the slider persistent
    showAllPopups: false, // to show all popups, instead of only one; same as the popup option "autoClose: false"
    showPopups: false // to set it so, when a marker is generated by the slider, its popup doesn't automatically display
});
```

Finally, when you added the line `$.getJSON("data/popups.json", function(json) {`, it required putting the rest of the `<script>` contents below it inside of this function. So this means you'll need to go to the last line of the `<script>` to close the curl bracket, the parentheses, and add the final semicolon:

```
    sliderControl1.startSlider();

});

</script>
```

(If adding the second dataset within the `<script>`, rather than via the external JSON file, do not do this last step of adding the closing curl bracket, perntheses, and semicolon.)

Once you've done all this, and your repo has been updated, you can once again go to your repo's webpage to see the slider in action. This time, it will display a number of markers simultaneously, then moving the slider will display the next set.

---
[Back to top](#leafletslider-tutorial)
