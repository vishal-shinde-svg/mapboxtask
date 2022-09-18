<template>
  <main class="w-screen h-screen">
    <div id="map"></div>
    <v-map class="w-full h-full" :options="state.map" @loaded="onMapLoaded">
      <div id="menu">
        <select id="layer-change">
          <option
            id="satellite-v9"
            type="checkbox"
            name="rtoggle1"
            value="satellite-v9"
            checked="checked"
          >
            satellite
          </option>
          <option
            id="dark-v10"
            type="checkbox"
            name="rtoggle2"
            value="dark-v10"
          >
            dark
          </option>
          <option
            id="light-v10"
            type="checkbox"
            name="rtoggle3"
            value="light-v10"
          >
            light
          </option>
          <option
            id="streets-v11"
            type="checkbox"
            name="rtoggle4"
            value="streets-v11"
          >
            streets
          </option>
          <option
            id="outdoors-v11"
            type="checkbox"
            name="rtoggle5"
            value="outdoors-v11"
          >
            outdoors
          </option>
        </select>
        <!-- <input id="satellite-v9" type="radio" name="rtoggle" value="satellite" checked="checked">
                <label for="satellite-v9">satellite</label>
                <input id="light-v10" type="radio" name="rtoggle" value="light">
                <label for="light-v10">light</label>
                <input id="dark-v10" type="radio" name="rtoggle" value="dark">
                <label for="dark-v10">dark</label>
                <input id="streets-v11" type="radio" name="rtoggle" value="streets">
                <label for="streets-v11">streets</label>
                <input id="outdoors-v11" type="radio" name="rtoggle" value="outdoors">
                <label for="outdoors-v11">outdoors</label> -->
      </div>
    </v-map>
  </main>
</template>
<script setup lang="ts">
import VMap from "v-mapbox";
import mapboxgl from "mapbox-gl";
const state = reactive({
  map: {
    accessToken:
      "pk.eyJ1Ijoic29jaWFsZXhwbG9yZXIiLCJhIjoiREFQbXBISSJ9.dwFTwfSaWsHvktHrRtpydQ",
    style: "mapbox://styles/mapbox/streets-v11?optimize=true",
    // style: "https://basemaps.cartocdn.com/gl/dark-matter-gl-style/style.json",
    center: [444.04931277036667, 26.266912177018096] as number[], //uses longitude, latitude
    zoom: 6,
    maxZoom: 22,
  },
});
function onMapLoaded(map) {
  ///---------Open marker on perticular location--------------
  // const marker = new mapboxgl.Marker({
  //     color: "#000000",
  //     draggable: true
  // }).setLngLat([30.5, 50.5])
  //     .addTo(map);
  // map.on('click', (e) => {
  //     console.log(e);
  //     console.log(e.lngLat)
  //     new mapboxgl.Marker({
  //         draggable: true,
  //         color: "#" + (Math.random().toString(16) + "000000").substring(2, 8),
  //     }).setLngLat([e.lngLat.lng, e.lngLat.lat]).addTo(map)
  //     new mapboxgl.Popup()
  //         .setLngLat([e.lngLat.lng, e.lngLat.lat])
  //         .setHTML("hello world")
  //         .addTo(map);
  // })
  // const layerList = document.getElementById('menu');
  // console.log(layerList);
  const layerToggle: any = document.getElementById("layer-change");
  // console.log(layerToggle.options[layerToggle.selectedIndex].value);
  layerToggle.addEventListener("change", (event) => {
    console.log(event);
    map.setStyle("mapbox://styles/mapbox/" + event.target.value);
  });
  // for (const input of inputs) {
  //     console.log("in for loop" + input);
  //     input.onclick = (layer) => {
  //         console.log("click on droupdown" + layer);
  //         const layerId = layer.target.id;
  //         const value = select.options[select.selectedIndex].value;
  //         map.setStyle('mapbox://styles/mapbox/' + layerId);
  //     }
  // }
}
</script>
<style>
body {
  margin: 0;
  padding: 0;
}
#map {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 100%;
}
#menu {
  position: absolute;
  background: #efefef;
  padding: 10px;
  font-family: "Open Sans", sans-serif;
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
.mapboxgl-popup {
  max-width: 400px;
  font: 12px/20px "Helvetica Neue", Arial, Helvetica, sans-serif;
}
</style>
