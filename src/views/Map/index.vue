<template>
  <div class="container_main_page">
    <div ref="basicMapbox" :style="mapSize"></div>
    <!-- <Mapboxtest mapWidth="100%" mapHeight="600px"></Mapboxtest> -->
    <h1>aowjefoaiwe</h1>
  </div>
</template>
<script>
import mapboxgl from 'mapbox-gl'
export default {
  name: "Mapboxtest",
  props: {
    mapWidth: {
      type: String
    },
    mapHeight: {
      type: String
    }
  },
  data () {
    return {
    }
  },
  mounted () {
    this.init()
  },
  methods: {
    // 初始化
    init () {
      mapboxgl.accessToken = 'pk.eyJ1IjoiamlhcWlzaGkxODciLCJhIjoiY2thdnI5cjRrMGh1bjJ6a3kwY2l2YWM0aiJ9.BOGjg2QBv1lFFcg4e6RYKA'
      const map = new mapboxgl.Map({
        container: this.$refs.basicMapbox,
        style: 'mapbox://styles/mapbox/dark-v10',
        center: [116.4, 40.6],
        zoom: 3
      })
      // San Francisco
    var origin = [116.4, 40.6];

    // Washington DC
    var destination = [23, 40.6];

    // A simple line from origin to destination.
    var route = {
        'type': 'FeatureCollection',
        'features': [
          {
            'type': 'Feature',
            'geometry': {
                'type': 'LineString',
                'coordinates': [origin, destination]
            }
          }
        ]
    };

    // A single point that animates along the route.
    // Coordinates are initially set to origin.
    var point = {
        'type': 'FeatureCollection',
        'features': [
          {
            'type': 'Feature',
            'properties': {},
            'geometry': {
                'type': 'Point',
                'coordinates': origin
            }
          }
        ]
    };

    // Calculate the distance in kilometers between route start/end point.
    var lineDistance = turf.lineDistance(route.features[0], 'kilometers');

    var arc = [];

    // Number of steps to use in the arc and animation, more steps means
    // a smoother arc and animation, but too many steps will result in a
    // low frame rate
    var steps = 500;

    // Draw an arc between the `origin` & `destination` of the two points
    for (var i = 0; i < lineDistance; i += lineDistance / steps) {
        var segment = turf.along(route.features[0], i, 'kilometers');
        arc.push(segment.geometry.coordinates);
    }

    // Update the route with calculated arc coordinates
    route.features[0].geometry.coordinates = arc;

    // Used to increment the value of the point measurement against the route.
    var counter = 0;
      console.log(map)
    }
  },
  computed: {
    mapSize () {
      let styleObj = {
        width: this.mapWidth,
        height: this.mapHeight
      }
      return styleObj
    }
  }
}

</script>
<style>
@import url('https://api.tiles.mapbox.com/mapbox-gl-js/v2.13.0/mapbox-gl.css');
</style>

