<html>
  <head>
    <title>ArcGIS REST with 512x512 Tiles</title>
    <link rel="stylesheet" href="https://openlayers.org/en/v4.6.4/css/ol.css" type="text/css">
    <!-- The line below is only needed for old environments like Internet Explorer and Android 4.x -->
    <script src="https://cdn.polyfill.io/v2/polyfill.min.js?features=requestAnimationFrame,Element.prototype.classList,URL"></script>
    <script src="https://openlayers.org/en/v4.6.4/build/ol.js"></script>
  </head>
  <body>
    <div id="map" class="map"></div>
    <script>
      var projection = ol.proj.get('EPSG:4326');

      // The tile size supported by the ArcGIS tile service.
      var tileSize = 512;

      var urlTemplate = 'https://services.arcgisonline.com/arcgis/rest/services/' +
          'ESRI_Imagery_World_2D/MapServer/tile/{z}/{y}/{x}';

      var map = new ol.Map({
        target: 'map',
        layers: [
          new ol.layer.Tile({
            source: new ol.source.XYZ({
              attributions: 'Copyright:© 2013 ESRI, i-cubed, GeoEye',
              maxZoom: 16,
              projection: projection,
              tileSize: tileSize,
              tileUrlFunction: function(tileCoord) {
                return urlTemplate.replace('{z}', (tileCoord[0] - 1).toString())
                    .replace('{x}', tileCoord[1].toString())
                    .replace('{y}', (-tileCoord[2] - 1).toString());
              },
              wrapX: true
            })
          })
        ],
        view: new ol.View({
          center: [39,-8],
          projection: projection,
          zoom: 6,
          minZoom: 2
        })
      });
    </script>
  </body>
</html>
