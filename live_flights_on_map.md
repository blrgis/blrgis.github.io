## Documentation for live flight details on a map

##### 1. Map Creation

Firstly, we have to insert OpenLayers css into an HTML file:

```html
<link rel="stylesheet"href="https://cdn.jsdelivr.net/gh/openlayers/openlayers.github.io@master/en/v6.1.1/css/ol.css"type="text/css">
```

Then we have to insert OpenLayers js file:

```html
<script src="https://cdn.jsdelivr.net/gh/openlayers/openlayers.github.io@master/en/v6.1.1/build/ol.js"></script>
```

Inside `body` of the html file, we have to create a `map` \<div>:

```html
<div id="map" class="map"></div>
``` 

In the above `map` \<div>, our map will reside.

Next, we will add an OSM map into the map:

```javascript
    <script type="text/javascript">
      var map = new ol.Map({
        target: 'map',
        layers: [
          new ol.layer.Tile({
            source: new ol.source.OSM()
          })
        ],
        view: new ol.View({
          center: ol.proj.fromLonLat([37.41, 8.82]),
          zoom: 4
        })
      });
    </script>
```


Final HTML will look like this:
```html
<!doctype html>
<html lang="en">
  <head>
    <link rel="stylesheet"href="https://cdn.jsdelivr.net/gh/openlayers/openlayers.github.io@master/en/v6.1.1/css/ol.css"type="text/css">
    <style>
      .map {
        height: 400px;
        width: 100%;
      }
    </style>
    <script src="https://cdn.jsdelivr.net/gh/openlayers/openlayers.github.io@master/en/v6.1.1/build/ol.js"></script>
    <title>OpenLayers example</title>
  </head>
  <body>
    <h2>My Map</h2>
    <div id="map" class="map"></div>
    <script type="text/javascript">
      var map = new ol.Map({
        target: 'map',
        layers: [
          new ol.layer.Tile({
            source: new ol.source.OSM()
          })
        ],
        view: new ol.View({
          center: ol.proj.fromLonLat([37.41, 8.82]),
          zoom: 4
        })
      });
    </script>
  </body>
</html>

```

##### 2. Placing the flight details

```javascript
 $.ajax({
                type: "GET",
                url: 'https://opensky-network.org/api/states/all?lamin=5&lomin=64&lamax=34&lomax=90',
                success: function (resp) {
                    resp.states.forEach(flight => {
                        marker.push(new ol.Feature({
                            geometry: new ol.geom.Point(
                                ol.proj.fromLonLat([flight[5], flight[6]])
                            ),
                            origin_country: flight[2],
                            altitude: flight[13]
                        })
                        );


                    });
                    var iconStyle = new ol.style.Style({
                        image: new ol.style.Icon(/** @type {olx.style.IconOptions} */({
                            anchor: [0.5, 0.5],
                            opacity: 0.75,
                            src: 'images/plane3.png',
                            // size: [10, 10],
                            // offset: [0,0],
                            // the scale factor
                            scale: 0.05
                        }))
                    });
                    var vectorSource = new ol.source.Vector({
                        features: marker
                    });
                    markerVectorLayer1 = new ol.layer.Vector({
                        source: vectorSource,
                        title: 'Flight',
                        style: iconStyle
                    });
                    map_project.addLayer(markerVectorLayer1);


                }
            });
``` 
