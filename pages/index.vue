<template>
  <main class="w-screen h-screen">
    <v-map class="w-full h-full" :options="state.map" @loaded="onMapLoaded" />
  </main>
</template>
<script setup lang="ts">
import mapboxgl from "mapbox-gl";
import VMap from "v-mapbox";
import geojson from "mapbox-gl";
const state = reactive({
  map: {
    // map: new mapboxgl.Map({
    //container: "map",
    accessToken:
      "pk.eyJ1IjoibWF5dXJ3YWtpa2FyIiwiYSI6ImNsNmdjdGxwbjBiNGMzY282bWh0dng2c2kifQ.y-m4-zQKOeOOnDG5I1u6ng",
    style: "mapbox://styles/mapbox/streets-v11?optimize=true",
    center: [75.9064, 17.6599],
    zoom: 11,
    maxZoom: 22,
    crossSourceCollisions: false,
    failIfMajorPerformanceCaveat: false,
    attributionControl: false,
    preserveDrawingBuffer: true,
    hash: false,
    minPitch: 0,
    maxPitch: 60,
  },
});
function onMapLoaded(map) {
  console.log("map load");
  const marker = new mapboxgl.Marker({
    color: "#FF0000",
    draggable: true,
  }).setLngLat([73.8743, 19.2032]);
  marker
    .setPopup(new mapboxgl.Popup().setHTML("<h1>Hello World!</h1>"))
    .addTo(map);
  marker.togglePopup();
  console.log("offset is" + marker.getOffset());
  console.log(marker.getPopup());

  // map.on('click', (e) => {
  //   console.log('I am clicked', e.lngLat.lat, e.lngLat.lng);
  //   new mapboxgl.Marker({
  //     draggable: true,
  //     color: '#000000'
  //   }).setLngLat([e.lngLat.lng, e.lngLat.lat]).addTo(map)

  map.flyTo({
    center: [73.8743, 19.2032],
    zoom: 9,
    speed: 0.4,
    curve: 1,
    easing(t) {
      const marker1 = new mapboxgl.Marker({
        color: "#FF0000",
        draggable: true,
      }).setLngLat([73.8743, 19.2032]);
      marker1.addTo(map);
      return t;
    },
  });
  //onclick
  marker.setLngLat([95.7129, 37.0902]);
  marker.addTo(map);

  map.on("click", (e) => {
    console.log(
      "i am clicked whin lngLat and lngLat",
      e.lngLat.lat,
      e.lngLat.lng
    );
    new mapboxgl.Marker({
      draggable: true,
      color: getRandomColor(),
    })
      .setLngLat([e.lngLat.lng, e.lngLat.lat])
      .setPopup(new mapboxgl.Popup().setHTML("<h1>hi</h1>"))

      .addTo(map);

    function getRandomColor() {
      var letters = "0123456789ABCDEF";
      var color = "#";
      for (var i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      return color;
    }
  });
  map.addControl(
    new mapboxgl.GeolocateControl({
      positionOptions: {
        enableHighAccuracy: true,
      },
      trackUserLocation: true,
      showUserHeading: true,
    })
  )
  // add layer
// const myScatterplotLayer = new MapboxLayer({
//   id: 'my-scatterplot',
//   type: ScatterplotLayer,
//   data: [
//       {position: [-74.5, 40], size: 100}
//   ],
//   getPosition: d => d.position,
//   getRadius: d => d.size,
//   getColor: [255, 0, 0]
// });

// // wait for map to be ready
// map.on('load', () => {
//   // insert before the mapbox layer "waterway_label"
//   map.addLayer(myScatterplotLayer, 'waterway_label');

//   // update the layer
//   myScatterplotLayer.setProps({
//     getColor: [0, 0, 255]
//   });
// });


}
</script>
<style>
html,
body {
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.w-screen {
  width: 100vw;
}
.h-screen {
  height: 100vh;
}
.h-full {
  height: 100%;
}
.w-full {
  width: 100%;
}
</style>