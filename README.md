# Mapping_Earthquakes

### Overview
Using JavaScript, Leaflet.js, and geoJSON data, the challenge was to build a map of the world which displayed every earthquake in the past seven days.

#### Deliverable 1
Added tectonic plate data to the map with the following code:

```
// 3. Use d3.json to make a call to get our Tectonic Plate geoJSON data.
  let tectonicData = "https://raw.githubusercontent.com/fraxen/tectonicplates/master/GeoJSON/PB2002_boundaries.json";
  
  d3.json(tectonicData).then(function(data) {
    L.geoJson(data,
      {color: "#B8860B",
      weight: 2
      }).addTo(tectonicPlates);
  });
});
```

#### Deliverable 2
Added major earthquake data as a third layer option to the map with the following code:

```
let majorEQs = "https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/4.5_week.geojson";
d3.json(majorEQs).then(function(data) {
  function styleInfo(feature) {
    return {
      opacity: 1,
      fillOpacity: 1,
      fillColor: getColor(feature.properties.mag),
      color: "#000000",
      radius: getRadius(feature.properties.mag),
      stroke: true,
      weight: 0.5
    };
  }

  function getColor(magnitude) {
    if (magnitude > 6) {
      return "#dc143c";
    }
    if (magnitude > 5) {
      return "#ffff00";
    }
    if (magnitude < 5) {
      return "#ff7f50";
    }
  }

  function getRadius(magnitude) {
    if (magnitude === 0) {
      return 1;
    }
    return magnitude * 4;
  }

  L.geoJson(data, {
    pointToLayer: function(feature, latlng) {
      console.log(data);
      return L.circleMarker(latlng);
    },
    style: styleInfo,
    onEachFeature: function(feature,layer) {
      layer.bindPopup("Magnitude: " + feature.properties.mag + "<br>Location: " + feature.properties.place);
    }
  }).addTo(majorEQ);
});
```

#### Deliverable 3
Added my choice of a third map style to the map with the following code:

```
// Deliverable 3: Create a third tile layer
let navNight = L.tileLayer('https://api.mapbox.com/styles/v1/mapbox/navigation-night-v1/tiles/{z}/{x}/{y}?access_token={accessToken}', {
	attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery (c) <a href="https://www.mapbox.com/">Mapbox</a>',
	maxZoom: 18,
	accessToken: API_KEY
});

// Create the map object with center, zoom level and default layer.
let map = L.map('mapid', {
	center: [40.7, -94.5],
	zoom: 3,
	layers: [streets]
});

// Create a base layer that holds all three maps.
let baseMaps = {
  "Streets": streets,
  "Satellite": satelliteStreets,
  "Night Navigation": navNight
};
```
