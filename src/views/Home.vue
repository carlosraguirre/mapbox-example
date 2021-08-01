<template>
  <div class="home">
    <h1>{{  message }}</h1>
    <div id='map' style='width: 600px; height: 500px;'></div>
  </div>
</template>

<style>
body { margin: 0; padding: 0; }
#map { position: absolute; top: 0; bottom: 0; width: 100%; }
</style>

<style>
  #marker {
    background-image: url('https://docs.mapbox.com/mapbox-gl-js/assets/washington-monument.jpg');
    background-size: cover;
    width: 50px;
    height: 50px;
    border-radius: 50%;
    cursor: pointer;
  }
  
  .mapboxgl-popup {
    max-width: 200px;
  }
</style>

<script>
import mapboxgl from 'mapbox-gl'; // or "const mapboxgl = require('mapbox-gl');"

export default {
  data: function () {
    return {
      message: "Welcome to Mapbox",
      places: [
        { lng: -77.0148, lat: 38.8891 , description: "Museum of planes, rockets, space and fun" },
        { lng: -77.0232, lat: 38.8920 , description: "Stuffed animals, old and new" }
      ]
    };
  },
  mounted: function() {
    this.launchMapbox();
  },
  methods: {
    launchMapbox: function () {
      mapboxgl.accessToken = process.env.VUE_APP_MAPBOX_API_KEY;
      var monument = [-77.0353, 38.8895];
      const map = new mapboxgl.Map({
        container: 'map', // container ID
        style: 'mapbox://styles/mapbox/streets-v11', // style URL
        center: monument, // starting position [lng, lat]
        zoom: 13 // starting zoom
      });
  
      var popup = new mapboxgl.Popup({ offset: 25 }).setText(
      'Construction on the Washington Monument began in 1848.'
      );

      // create DOM element for the marker
      var el = document.createElement('div');
      el.id = 'marker';

      // create the marker
      new mapboxgl.Marker(el)
        .setLngLat(monument)
        .setPopup(popup) // sets a popup on this marker
        .addTo(map);

      // loop through places and make a new marker for each element
      this.places.forEach(function(place) {
        console.log(place)

        //create the popup
        var popup = new mapboxgl.Popup({ offset: 25 }).setText(
        place.description
        );
        new mapboxgl.Marker()
          .setLngLat([place.lng, place.lat])
          .setPopup(popup) // sets a popup on this marker
          .addTo(map);

      var marker1 = new mapboxgl.Marker()
        .setLngLat([-77.0340, 38.8984])
        .addTo(map);
      })
    },
  }
}
</script>
