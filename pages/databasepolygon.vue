<template>
  <div>
    <main class="w-screen h-screen">
      <head>
        <link
          href="https://api.mapbox.com/mapbox-gl-js/v2.10.0/mapbox-gl.css"
          rel="stylesheet"
        />
      </head>
      <v-map class="w-full h-full" :options="state.map" @loaded="onMapLoaded" />
      <div id="menu" class="">
        <input
          id="satellite-v9"
          type="radio"
          name="rtoggle"
          value="satellite"
          checked="checked"
        />
        <label for="satellite-v9">satellite</label>
        <input id="light-v10" type="radio" name="rtoggle" value="light" />
        <label for="light-v10">light</label>
        <input id="dark-v10" type="radio" name="rtoggle" value="dark" />
        <label for="dark-v10">dark</label>
        <input id="streets-v11" type="radio" name="rtoggle" value="streets" />
        <label for="streets-v11">streets</label>
        <input id="outdoors-v11" type="radio" name="rtoggle" value="outdoors" />
        <label for="outdoors-v11">outdoors</label>
      </div>
    </main>
  </div>
</template>
<script setup lang="ts">
import MapboxGeocoder from "@mapbox/mapbox-gl-geocoder";
import vMap from "v-mapbox";
import mapboxgl from "mapbox-gl";
import geojson from "geojson";
import { once } from "events";
//import globe from "globe"
mapboxgl.accessToken =
  "pk.eyJ1Ijoic29jaWFsZXhwbG9yZXIiLCJhIjoiREFQbXBISSJ9.dwFTwfSaWsHvktHrRtpydQ";
//const data: string;
const state = reactive({
  map: {
    container: "map",
    zoom: 5,
    center: [74.04931277036667, 18.266912177018096],

    style: "mapbox://styles/mapbox-map-design/ckhqrf2tz0dt119ny6azh975y",
  },
});
function onMapLoaded(map: mapboxgl.Map) {
  line();
  polygon();
  async function polygon() {
    state.polygon = await $fetch("http://localhost:8080/map/geom");
    console.log(state.polygon);
    console.log("data: ", state.polygon);

    const featureCollection = {
      type: "geojson",
      data: {
        type: "FeatureCollection",
        features: [],
      },
    };

    featureCollection.data.features = state.polygon.map((element) => {
      return {
        type: "Feature",
        geometry: {
          type: "Polygon",
          coordinates: element.geometry.coordinates,
        },
      };
    });

    map.addSource("polygon-data", featureCollection);

    map.addLayer({
      id: "park-boundary",
      type: "fill",
      source: "polygon-data",
      paint: {
        "fill-color": "#A020F0",
        "fill-opacity": 0.4,
      },
    });

    // map.addLayer({
    //   id: "outline",
    //   type: "line",
    //   source: "park-boundary",
    //   layout: {},
    //   paint: {
    //     "line-color": "#000",
    //     //dark blackish
    //     "line-width": 3,
    //   },
    // });
  }
  //////////////////////////////////////Line String data come from databse//////////////////////////
  line();
  async function line() {
    state.line = await $fetch("http://localhost:8080/map/geom");
    console.log(state.line);
    console.log("data: ", state.line);

    const featureCollectionline = {
      type: "geojson",
      data: {
        type: "FeatureCollection",
        features: [],
      },
    };

    featureCollectionline.data.features = state.line.map((element) => {
      return {
        type: "Feature",
        geometry: {
          type: "LineString",
          coordinates: element.geometry.coordinates,
        },
      };
    });

    map.addSource("line-data", featureCollectionline);

    // map.addLayer({
    //   id: "park",
    //   type: "fill",
    //   source: "line-data",
    //   paint: {
    //     "fill-color": "#A020F0",
    //     "fill-opacity": 0.4,
    //   },
    // });
    map.addLayer({
      id: "route",
      type: "line",
      source: "line-data",
      layout: {
        "line-join": "round",
        "line-cap": "round",
      },
      paint: {
        "line-color": "#888",
        "line-width": 4,
      },
    });
    // map.addLayer({
    //   id: "outline",
    //   type: "line",
    //   source: "park-boundary",
    //   layout: {},
    //   paint: {
    //     "line-color": "#000",
    //     //dark blackish
    //     "line-width": 3,
    //   },
    // });
  }
}
</script>
<style>
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
  max-width: 200px;
}
.mapboxgl-popup-content {
  text-align: center;
  font-family: "Open Sans", sans-serif;
}
#menu {
  position: absolute;
  background: #efefef;
  padding: 10px;
  font-family: "Open Sans", sans-serif;
}
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
