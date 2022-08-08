<template>
  <main class="w-screen h-screen">
    <v-map class="w-full h-full" :options="state.map" />
  </main>
</template>
<script>
import VMap from "v-mapbox";
import { reactive } from "vue";
export default {
  name: "App",
  components: {
    VMap,
  },
  setup() {
    const state = reactive({
      map: {
        accessToken:
          "pk.eyJ1IjoidmlzaGFsc2hpbmRlMTIzIiwiYSI6ImNsNmdjd2ZrNTF1M2EzaXFsMGdjb2x5ZzMifQ.x4GsVU9Jwc1Slc65ahnsPA",
        style: "mapbox://styles/mapbox/streets-v11?optimize=true",
        // style: "https://basemaps.cartocdn.com/gl/dark-matter-gl-style/style.json",
        center: [73.8567, 18.5204],
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
    return {
      state,
    };
  },
};
</script>
<style>
/* @import "~mapbox-gl/dist/mapbox-gl.css";
@import "~v-mapbox/dist/v-mapbox.css"; */
html,
body {
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2C3E50;
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