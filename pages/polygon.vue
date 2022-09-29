<template>
  <div>
    <div class="main-div">
      <div class="left-div">
        <h3>Data</h3>
        <ul>
          <li v-for="(item, index) in state.layers" :key="index">
            <input type="checkbox" @click="getdata(index)" />
            {{ item.name }}
          </li>
        </ul>
      </div>
      <div class="right-div">
        <v-map :options="state.map" @loaded="onMapLoaded"></v-map>
        <nav id="menu"></nav>
        <div id="map"></div>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import VMap from "v-mapbox";
import mapboxgl from "mapbox-gl";
import "@mapbox/mapbox-gl-draw/dist/mapbox-gl-draw.css";
import MapboxDraw from "@mapbox/mapbox-gl-draw";
const state = reactive({
  map: {
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
  layers: [],
  layersNmae: "",
});
function getdata(e) {
  console.log(state.layers[0]);
}
function onMapLoaded(map: mapboxgl.Map) {
  //   var Draw = new MapboxDraw();
  //     map.addControl(Draw, "top-left");
  map.addControl(new mapboxgl.NavigationControl());
  var Draw = new MapboxDraw();
  map.addControl(Draw, "top-left");
  map.on("draw.create", updateArea);
  //   function updateArea(e) {
  //     console.log(e);
  //     state.layearName = state.layearName + 1;
  //     console.log(e.features[0].geometry);
  //     // create object
  //     //     let coordinates = e.features[0].geometry.coordinates;
  //     //     // let type1 = e.features[0].geometry.type;
  //     //     let data = { name: state.layearName };
  //     //     // data push in array
  //     //     state.layears.push(state);
  //     //     console.log(coordinates);
  //     //   }
  //     //   console.log(state.layers);
  //     let coordinate = e.features[0].geometry;
  //     let arr = coordinate.push(state.layers);
  //     console.log(arr);
  //   }
  function updateArea(e) {
    // state.layearName = state.layearName ;
    console.log(e.features[0].geometry);
    // create object
    let name = prompt("Enter the name ");
    
    let coordinates = e.features[0].geometry.coordinates;
    let type1 = e.features[0].geometry.type;
    let data1 = {
      name: `${name}`,
      type: `${type1}`,
      coordinates: `${coordinates}`,
    };
    // const popup = new mapboxgl.Popup()
    //   .setLngLat(coordinates)
    //   .setHTML(`<input type="text" v-model="data.layearName"/> `)
    //   .addTo(map);
    // data push in array
    state.layers.push(data1);
    console.log(coordinates);
    console.log(type1);
  }
  console.log(state.layers);
}
</script>
<style>
.left-div {
  height: 100vh;
  width: 20vw;
  background-color: white;
  float: left;
}
.right-div {
  height: 100vh;
  width: 80vw;
  float: right;
}
.main-div {
  height: 100vh;
  width: 100vw;
}
</style>
