<template>
  <main class="w-screen h-screen">
    <v-map class="w-full h-full" :options="state.map" @loaded="onMapLoaded">
      <div id="menu">
        <select id="layer-change">
          <option
            id="satellite-v9"
            type="checkbox"
            name="style1"
            value="satellite-v9"
            checked="checked"
          >
            satellite
          </option>
          <option id="dark-v10" type="checkbox" name="style2" value="dark-v10">
            dark
          </option>
          <option
            id="light-v10"
            type="checkbox"
            name="style3"
            value="light-v10"
          >
            light
          </option>
          <option
            id="streets-v11"
            type="checkbox"
            name="style4"
            value="streets-v11"
          >
            streets
          </option>
          <option
            id="outdoors-v11"
            type="checkbox"
            name="style5"
            value="outdoors-v11"
          >
            outdoors
          </option>
        </select>
      </div>
    </v-map>
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
  ),
    map.addLayer({
      id: "points-of-interest",
      source: {
        type: "vector",
        url: "mapbox://mapbox.mapbox-streets-v8",
      },
      "source-layer": "poi_label",
      type: "circle",
      paint: {
        // Mapbox Style Specification paint properties
      },
      layout: {
        // Mapbox Style Specification layout properties
      },
    }),
    map.on("load", () => {
      const layers1 = map.getStyle().layers;
      // Find the index of the first symbol layer in the map style.
      let firstSymbolId;
      for (const layer of layers1) {
        if (layer.type === "symbol") {
          firstSymbolId = layer.id;
          break;
        }
      }

      map.addSource("urban-areas", {
        type: "geojson",
        data: "https://docs.mapbox.com/mapbox-gl-js/assets/ne_50m_urban_areas.geojson",
      });
      map.addLayer(
        {
          id: "urban-areas-fill",
          type: "fill",
          source: "urban-areas",
          layout: {},
          paint: {
            "fill-color": "#f08",
            "fill-opacity": 0.4,
          },
          // This is the important part of this example: the addLayer
          // method takes 2 arguments: the layer as an object, and a string
          // representing another layer's name. If the other layer
          // exists in the style already, the new layer will be positioned
          // right before that layer in the stack, making it possible to put
          // 'overlays' anywhere in the layer stack.
          // Insert the layer beneath the first symbol layer.
        },
        firstSymbolId
      );
    });

  const layerToggle: any = document.getElementById("layer-change");
  // console.log(layerToggle.options[layerToggle.selectedIndex].value);
  layerToggle.addEventListener("change", (event) => {
    console.log(event);
    map.setStyle("mapbox://styles/mapbox/" + event.target.value);
  });
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
