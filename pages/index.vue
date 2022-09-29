<template>
  <!-- <div class="grid grid-cols-2 divide-x"> -->
  <!-- <div class="text-slate-100 text-lg bg-green-700 font-bold h-10 w-10">1</div> -->
  <div>
    <v-map class="w-full h-full" :options="state.map" @loaded="onMapLoaded" />
  </div>
  <!-- </div> -->
</template>
<script setup lang="ts">
import MapboxDraw from "@mapbox/mapbox-gl-draw";
import "@mapbox/mapbox-gl-draw/dist/mapbox-gl-draw.css";
import MapboxGeocoder from "@mapbox/mapbox-gl-geocoder";
import vMap from "v-mapbox";
import mapboxgl from "mapbox-gl";
//import geojson from "geojson";
//import { once } from "events";
//import globe from "globe"
mapboxgl.accessToken =
  "pk.eyJ1Ijoic29jaWFsZXhwbG9yZXIiLCJhIjoiREFQbXBISSJ9.dwFTwfSaWsHvktHrRtpydQ";

//const data: string;
const state = reactive({
  map: {
    container: "map",
    zoom: 14,
    center: [-87.61694, 41.86625],
    //pitch: 45,
    // pitch: 85,
    // bearing: 80,
    // Choose from Mapbox's core styles, or make your own style with Mapbox Studio
    style: "mapbox://styles/mapbox/satellite-streets-v11",
    // zoom: 15,
    // pitch: 55,
    // bearing: 20,
    // antialias: true,
    center: [441.5625, 25.799891182088334] as number[],
    zoom: 2.5,
    maxZoom: 22,
  },
});
function onMapLoaded(map: mapboxgl.Map) {
  var Draw = new MapboxDraw();
  map.addControl(Draw, "top-left");
  map.on("dblclick", (e) => {
    new mapboxgl.Marker({
      color: "#" + (Math.random().toString(16) + "87CEEB").substring(2, 8),
      draggable: true,
    })
      .setLngLat([e.lngLat.lng, e.lngLat.lat])
      .addTo(map);
    console.log(`A click event has occurred at ${e.lngLat}`);
  });
}
</script>
<style scoped>
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
</style>
