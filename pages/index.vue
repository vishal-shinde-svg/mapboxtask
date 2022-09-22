<template>
  <head>
    <link
      rel="stylesheet"
      href="https://api.mapbox.com/mapbox-gl-js/plugins/mapbox-gl-draw/v1.3.0/mapbox-gl-draw.css"
      type="text/css"
    />
  </head>
  <main class="w-screen h-screen">
    <!-- <div id="map"></div> -->
    <v-map class="w-full h-full" :options="state.map" @loaded="onMap">
      <div id="menu">
        <div class="pre">
          <pre id="info"></pre>
        </div>
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
// display map on Browser......................................
import mapboxgl from "mapbox-gl";
import MapboxGeocoder from "@mapbox/mapbox-gl-geocoder";
import VMap from "v-mapbox";
import MapboxDraw from "@mapbox/mapbox-gl-draw";

mapboxgl.accessToken =
  "pk.eyJ1Ijoic29jaWFsZXhwbG9yZXIiLCJhIjoiREFQbXBISSJ9.dwFTwfSaWsHvktHrRtpydQ";
const state = reactive({
  map: {
    container: "map",
    style: "mapbox://styles/mapbox/streets-v11?optimize=true",
    center: [73.856743, 18.52043] as number[],
    zoom: 6,
    maxZoom: 22,
  },
});

// To get Backend point in frontend.....................................

// let mapData: any = await $fetch("http://localhost:3000/map/");
// console.log("map data", mapData);

// add different style on map ..................................
async function onMap(map: mapboxgl.Map) {
  const layerToggle: any = document.getElementById("layer-change");
  layerToggle.addEventListener("change", (event) => {
    console.log(event);
    map.setStyle("mapbox://styles/mapbox/" + event.target.value);
  });

  //search bar used geocoder to display search bar.........

  map.addControl(
    new MapboxGeocoder({
      accessToken: mapboxgl.accessToken,
      mapboxgl: mapboxgl,
    })
  );

  //marker..................................
  // mapData.map((ele) => {
  //   new mapboxgl.Marker({
  //     draggable: true,
  //     color: "#" + (Math.random().toString(16) + "000000").substring(2, 8),
  //   })
  //     .setLngLat([ele.lat, ele.lon])
  //     .addTo(map);
  // });

  // add polygon on pune location..........................

  map.addSource("pune", {
    type: "geojson",
    data: {
      type: "Feature",
      geometry: {
        type: "Polygon",
        coordinates: [
          [
            [73.68942260742188, 18.530398219358684],
            [73.65509033203125, 18.340187242207897],
            [73.99154663085938, 18.359739156553683],
            [73.99429321289062, 18.641040231399984],
            [73.68942260742188, 18.530398219358684],
          ],
        ],
      },
    },
  });
  map.addLayer({
    id: "pune",
    type: "fill",
    source: "pune", // reference the data source
    layout: {},
    paint: {
      "fill-color": " red", // blue color fill
      "fill-opacity": 1,
    },
  });

  // fly the map and add the the marker at center position............................
  map.flyTo({
    center: [73.8743, 19.2032],
    zoom: 9,
    speed: 0.9,
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

  //---------------------current location-------------------

  map.addControl(
    new mapboxgl.GeolocateControl({
      positionOptions: {
        enableHighAccuracy: true,
      },
      trackUserLocation: true,
      showUserHeading: true,
    })
  );

  //if you click add marker..............................................

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
  //.....................................................
  //add ploygon using backend

  let allStude = reactive({
    mapData: [],
  });
  allStude.mapData = await $fetch("http://localhost:3000/map/");
  console.log(allStude.mapData);

  allStude.mapData.map((ele) => {
    new mapboxgl.Marker({
      draggable: true,
      color:  "#" + (Math.random().toString(16) + "000000").substring(2, 8),
    })
      .setLngLat([ele.lat, ele.lon])
      .addTo(map);
  });

  let pointsData1 = [];
  allStude.mapData.map((ele) => {
    let arrFormat = [ele.lat, ele.lon];
    pointsData1.push(arrFormat);
  });
  console.log(pointsData1);
  map.addSource("back", {
    type: "geojson",
    data: {
      type: "Feature",
      geometry: {
        type: "Polygon",
        coordinates: [pointsData1],
      },
    },
  });
  map.addLayer({
    id: "back",
    type: "line",
    source: "back",
    layout: {
      "line-join": "round",
      "line-cap": "round",
    },
    paint: {
      "line-color": "red",
      "line-width": 1,
      "line-opacity": 1,
    },
  });
  map.addLayer({
    id: "back",
    type: "fill",
    source: "pune",
    layout: {},
    paint: {
      "fill-color": "black",
      "fill-opacity": 0.5,
    },
  });


  //.........................Draw tool...............
  var Draw = new MapboxDraw();
  map.addControl(Draw, "top-right");

  map.on("mousemove", (e) => {
    document.getElementById("info").innerHTML =
      // `e.point` is the x, y coordinates of the `mousemove` event
      // relative to the top-left corner of the map.
      JSON.stringify(e.point) +
      "<br />" +
      // `e.lngLat` is the longitude, latitude geographical position of the event.
      JSON.stringify(e.lngLat.wrap());
  });

}
</script>
<style>
body {
  margin: 0;
  padding: 0;
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
.pre {
  z-index: 1;
  position: relative;

  left: 600px;
  width: 40%;
  height: 9%;
  background-color: white;
  top: 0;
}
</style>