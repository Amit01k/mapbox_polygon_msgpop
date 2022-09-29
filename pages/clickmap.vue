<template>
  <div class="main-div">
    <div class="left-div">
      <ul>
        <li v-for="(item, index) in state.arr" :key="index">
          <input type="checkbox" @click="getdata(index)" />
          {{ item.name }}
        </li>
      </ul>
    </div>

    <div class="right-div">
      <v-map class="w-full h-full" :options="state.map" @loaded="onMapLoaded" />
    </div>
    <div>
      <p>city name{{ state.polygonDetails.name }}</p>
    </div>
  </div>

  <!-- </div> -->
</template>
<script setup lang="ts">
import MapboxDraw from "@mapbox/mapbox-gl-draw";
import "@mapbox/mapbox-gl-draw/dist/mapbox-gl-draw.css";
import MapboxGeocoder from "@mapbox/mapbox-gl-geocoder";
import vMap from "v-mapbox";
import mapboxgl from "mapbox-gl";

mapboxgl.accessToken =
  "pk.eyJ1Ijoic29jaWFsZXhwbG9yZXIiLCJhIjoiREFQbXBISSJ9.dwFTwfSaWsHvktHrRtpydQ";

//const data: string;
const state = reactive({
  map: {
    container: "map",

    style: "mapbox://styles/mapbox/satellite-streets-v11",

    center: [-100.04, 38.907] as number[],
    zoom: 2.5,
    maxZoom: 22,
  },
  arr: [],
  polygonDetails: {
    name: "",
    desc: "",
  },
});

console.log("this is come from line nu 54", state.polygonDetails);

function getdata(e) {
  console.log(state.arr[0]);
}
function onMapLoaded(map: mapboxgl.Map) {
  const draw = new MapboxDraw({
    displayControlsDefault: false,

    controls: {
      polygon: true,
      trash: true,
    },

    defaultMode: "draw_polygon",
  });
  map.addControl(draw, "top-left");

  map.on("draw.create", updateArea);
  map.on("draw.delete", updateArea);
  map.on("draw.update", updateArea);

  function updateArea(e) {
    let name = prompt("Enter the name ");
    let description = prompt("Enter desription about location");

    let coordinates = e.features[0].geometry.coordinates;
    let type1 = e.features[0].geometry.type;
    let polygon = {
      name: `${name}`,
      description: `${description}`,
      //data: `${x}`,
      type: `${type1}`,
      coordinates: `${coordinates}`,
      details: `${state.polygonDetails}`,
    };

    state.arr.push(polygon);
    console.log(coordinates);
    console.log(type1);
  }
  console.log("line nu 148", state.polygonDetails);
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
.main-div {
  height: 100vh;
  width: 100vw;
}
.left-div {
  height: 100vh;
  width: 20vw;
  background-color: aquamarine;
  float: left;
}
.right-div {
  height: 100vh;
  width: 80vw;
  float: right;
}
.mapboxgl-popup {
  max-width: 200px;
}
</style>
