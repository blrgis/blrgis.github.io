# Documentation for visualising live flight location on an OSM map

| [Arnab Dutta](https://arnabdutta73.github.io/) |&nbsp;<a href="https://twitter.com/Fa7C0n"><img alt="SVG" src="/icons/Twitter_Social_Icon_Circle_Color.svg" width="17px" height="17px"> &nbsp;<a href="https://www.linkedin.com/in/arnab-dutta/"><img alt="PNG" src="/icons/icons8-linkedin.svg" width="20px" height="20px"> &nbsp;[:octocat: ](https://github.com/arnabdutta73) &nbsp;[:email: ](mailto:arnabdutta73@gmail.com)|


`*Note: Article assumes HTML basic knowledge as a prequisite.`

##### 1. Map Creation

After balancing out the pros and cons of Openlayers, Leaflet, and mapbox, I decided to go with OpenLayers.
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
`Note: the <div> tag containg 'map' id should be the same everywhere in the code. So, if you chose an id as 'MyMap', just put 'MyMap' anywhere you want to use the object.`


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
```javascript
new ol.Map: it is the function call to add a map object from openlayers to your target map div.
layers: this will contain all the names of the layers which will sit in your target map div.
new ol.view: sets the default center and zoom values for the map.
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

AJAX library is used to fetch live flight details from a URL.
[opensky](https://opensky-network.org/) website gives the details of the flights. I used an API with URL: 'https://opensky-network.org/api/states/all?lamin=5&lomin=64&lamax=34&lomax=90' to fetch the details using GET request. (For the HTTP requests, read it [here](https://www.w3schools.com/xml/xml_http.asp).)

'success' is used when the api returns a successfull request.

'marker' is used to display an icon over the flights.

'addLayer' adds the given layer to the list of layers that will be in the map div.

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
