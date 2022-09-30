<template>
  <div>
    <div class="flex">
      <!-- <div class="form-class"> -->
      <form v-if="state.flag" v-on:submit.prevent class="form-class">
        <tr>
          <td>
            Name:<input type="text" id="name" v-model="states.obj.name" />
          </td>
        </tr>
        <br />
        <!-- <tr>
          <td>description: <input type="text" /></td>
        </tr> -->
        <button type="submit" @click="submitForm">submit</button>
      </form>
      <!-- </div> -->
      <main class="w-screen h-screen">
        <v-map
          class="w-full h-full"
          :options="state.options"
          @loaded="onMapLoaded"
        />
        <div id="calculated-area"></div>
      </main>
      <div class="dislpay-arena">
        <!-- <div>hiii{{ state.data2 }}</div> -->
        <input type="checkbox" />{{ state.data2 }}
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import VMap from "v-mapbox";
import "@mapbox/mapbox-gl-draw/dist/mapbox-gl-draw.css";
import { ref } from "vue";
import mapboxgl from "mapbox-gl";
import MapboxDraw from "@mapbox/mapbox-gl-draw";
const name = ref("");
const state = reactive({
  options: {
    accessToken:
      "pk.eyJ1IjoicGF1bDAyIiwiYSI6ImNsNmdnY2p0MTE3MTUzY24yd3d3djZ0cnYifQ.IwKuQR_eGwyojW3tckCTVQ",
    style: "mapbox://styles/mapbox/streets-v11?optimize=true",
    center: [73.8567, 18.5204],
    zoom: 11,
  } as mapboxgl.MapboxOptions,
  data2: [],
  flag: false,
});

const states: any = {
  //   name: [],
  obj: {
    name: "",
  },
};

console.log("this is come from line nu-58 ", states.name);
//console.log("for properties", state.data2.features.geometry[0].properties);

function submitForm() {
  //   state.data2.push(states.name);
  state.data2.push(states.obj);
  console.log("submit date" + state.data2);
}

// var coordinates = [-77.03171253204346, 38.91457788952212];
// var description = "<button onclick=' + { this.handlePop } + '> Button</button>"
function onMapLoaded(map: mapboxgl.Map) {
  var Draw = new MapboxDraw({
    displayControlsDefault: true,
    controls: {
      polygon: true,
      trash: true,
      line_string: true,
    },
  });
  map.addControl(Draw, "top-left");
  map.on("draw.create", updateArea);
  // Change the cursor to a pointer when the mouse is over the places layer.
  map.on("mouseenter", "places", function () {
    map.getCanvas().style.cursor = "pointer";
  });
  // Change it back to a pointer when it leaves.
  map.on("mouseleave", "places", function () {
    map.getCanvas().style.cursor = "";
  });
  function updateArea(e) {
    console.log("thi is come from line nu-88", state.data2);

    state.flag = true;
    console.log("clicked");
    const data = Draw.getAll();
    state.data2.push(data);
    // state.flag = false;
  }
  state.flag = false;
}
</script>
<style>
html,
body {
  margin: 0;
}
.flex {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin: 0%;
}
.w-screen {
  width: 75%;
}
.h-screen {
  height: 100vh;
}

.dislpay-arena {
  background: #fdfafae9;
  box-shadow: 0px -3px 10px rgba(0, 58, 78, 0.1);
  border-radius: 5px;
  padding: 20px 30px;
  width: 35%;
}
.calculation-box {
  height: 75px;
  width: 150px;
  position: absolute;
  bottom: 40px;
  left: 10px;
  background-color: rgba(255, 255, 255, 0.9);
  padding: 15px;
  text-align: center;
}
.form-class {
  top: 50px;
  left: 250px;
  background-color: aqua;
  width: 300px;
  border-radius: 10px;
  height: 100px;
  position: absolute;
  z-index: 2;
  padding: 5px;
}
.form-class,
p {
  color: rgb(54, 141, 248);
  padding-left: 10px;
}
button {
  color: red;
  margin-top: 8px;
  align-items: center;
  text-align: center;

  background-color: green;
  border-block-start-color: tomato;
  height: 20px;
  width: 40px;
  /* border: 2px solid pink; */
}
input {
  /* pa */
}
</style>
