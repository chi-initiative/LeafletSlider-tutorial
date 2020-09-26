# LeafletSlider-tutorial
This is a tutorial repo to introduce people to using Leaflet Time-Slider plugin in a Leaflet map. Fork this repo and then work through this tutorial by writing into the blank html file.

## Set up html file for map
Begin by setting up your html file with the basic structure.

```html
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
    var map1 = L.map('myMap').setView([46.25,-119.17], 11);

    // Add tileLayer to map1
    L.tileLayer('https://{s}.tile.openstreetmap.fr/hot/{z}/{x}/{y}.png', {
        attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
    }).addTo(map1);
</script>
```

After adding this code to your file, you can go to your repo's Settings tab, scroll down to the GitHub Pages section, choose "master" from the Sources dropdown list, and click "Save." Keep refreshing the page until this same section has a green bar with the text, "Your site is published at..." You can click on this link to view your page, with the map loaded. If you go to this link and the only thing that loads is the text, "If you can read this, either an error has occurred, or you're getting cute with the developer tools of your browser", then go back through these first few steps to confirm you have closely followed them.

If the page loads with a map, then your HTML file is properly set up.

## Add resources for the plugin
The Leaflet Time-Slider module is meant to control the display of markers, one at a time, via sliding control. To accomplish this requires adding additional JS scripts to your `<head>` section. One of these, the jquery UI script, is available via jquery's CDN:

```html
<!--jquery UI-->
<script src="https://code.jquery.com/ui/1.12.1/jquery-ui.js" integrity="sha256-T0Vest3yCU7pafRw9r+settMBX6JkKN06dqBnpQ8d30=" crossorigin="anonymous"></script>
```

You'll also need to add the module's locally-saved JS script, which is inside the "js" folder.

```html
<!--Leaflet Time-Slider module script;
find maintained version at https://github.com/Falke-Design/LeafletSlider-->
<script src="js/SliderControl.js"></script>
```

## Add popups
Adding a few popups directly within the html file will serve as a basic example of how this module works.

Begin with a list of a few markers, added as a set of geoJSON-styled data. As a reminder, geoJSON files put the longitude first and latitude second as the coordinates, so these are reversed from how they are usually read.

Add the following into the `<script>` section, where you previously put the map and tile layer:

```javascript
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
            time: '1992/06'
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
            time: '1992/07'
        }
    }
];
```

After copying in this geoJSON-formatted data, you'll need create a new variable, which will define the popup structure that will be attached to each marker (via a second variable). This code will reference different parts of the "properties" spelled out in the data. It needs to be added *below* the geoJSON data, map, and tile layer portions of the code:

```javascript
var optionsObject1 = {
    onEachFeature: onEachFeature
};
function onEachFeature (feature, layer) {
    var content = "<div style='clear: both'></div><div><h4>" + feature.properties.title + "</h4><p>" + feature.properties.time + "</p><p>" + feature.properties.description + "</p></div>";
    layer.bindPopup(content, {closeButton: true});
}
```

The second variable, `content`, describes which parts of the geoJSON data fits into which part of the popup that will be generated. Each popup is bound to each marker, added to the existing map as a new layer. When binding the popup, it also adds a close-popup button in its deault location (the top-right of the popup).

Under where this has all been specified, add this new variable, which defines a group of geoJSON-defined markers and the associated popups, based upon the data.

```javascript
var group1 = L.geoJSON(dataset1, optionsObject1);
```

To view these markers and popups, add this line, once again below the previous line:

```javascript
group1.addTo(map1);
```

Once GitHub updates with this most recent change, you can load your repo's website and see these markers on your map. Click on each of them to see their unique popups.

## Enable the Slider

To enable the slider, first delete the last line added:

<code><del>group1.addTo(map1);</del></code>
