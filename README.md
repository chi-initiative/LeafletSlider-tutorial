# LeafletSlider-tutorial
This is a tutorial repo to introduce people to using Leaflet Time-Slider plugin in a Leaflet map.

## Set up html file
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
<div id="map">If you can read this, either an error has occurred, or you're getting cute with the developer tools of your browser.</div>
```

Still inside `<body>`, create a script block, for creating the map. Set its view however you'd like; or just use the one I've defaulted to. Load a tile layer with one of your choice; for instance, this example uses one from [OpenStreetMap.org](https://www.openstreetmap.org/). It's also always a good idea to include the

```html
<script>
    // Create map (map1) inside div with #myMap ID; set the view
    var map1 = L.map('myMap').setView([46.25,-119.17], 11);

    // Add tileLayer to map1
    L.tileLayer('https://{s}.tile.openstreetmap.fr/hot/{z}/{x}/{y}.png', {
        attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
    }).addTo(map1);
</script>
```
