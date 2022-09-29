<template>
  <div>
    <div class="flex">
      <!-- <div class="form-class"> -->
      <form v-if="data1.flag" v-on:submit.prevent class="form-class">
        <tr>
          <td>Name:<input type="text" id="name" /></td>
        </tr>
        <br />
        <tr>
          <td>description: <input type="text" /></td>
        </tr>
        <button type="submit">submit</button>
      </form>
      <!-- </div> -->
      <main class="w-screen h-screen">
        <v-map
          class="w-full h-full"
          :options="data1.options"
          @loaded="onMapLoaded"
        />
        <div id="calculated-area"></div>
      </main>
      <div class="dislpay-arena">
        <div>{{ data1.data2 }}</div>
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
const data1 = reactive({
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
    styles: [
      //linestring style
      {
        id: "gl-draw-line",
        type: "line",
        filter: [
          "all",
          ["==", "$type", "LineString"],
          ["!=", "mode", "static"],
        ],
        layout: {
          "line-cap": "round",
          "line-join": "round",
        },
        paint: {
          "line-color": "#0254A2",
          "line-dasharray": [0.2, 2],
          "line-width": 5,
        },
      },
      //polygon stroke style
      {
        id: "gl-draw-polygon-stroke-active",
        type: "line",
        filter: ["all", ["==", "$type", "Polygon"], ["!=", "mode", "static"]],
        layout: {
          "line-cap": "round",
          "line-join": "round",
        },
        paint: {
          "line-color": "#0254A2",
          "line-dasharray": [0.2, 2],
          "line-width": 5,
        },
      },
      // polygon fill style
      {
        id: "gl-draw-polygon-fill",
        type: "fill",
        filter: ["all", ["==", "$type", "Polygon"], ["!=", "mode", "static"]],
        paint: {
          "fill-color": "#0254A2",
          "fill-outline-color": "#0254A2",
          "fill-opacity": 0,
        },
      },
    ],
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
    data1.flag = true;
    console.log("clicked");
    const data = Draw.getAll();
    data1.data2.push(data);
    map.on("click", "gl-draw-polygon-fill.cold", function (e) {
      console.log(e.lngLat);
      //   var description =
      //     "<form> Enter name: <input type='text' id='name' /></br> Enter Description:<input type='text' id='description'/><button id='view-full'>Submit</button></form > ";
      let popupElem = new mapboxgl.Popup({ focusAfterOpen: true })
        .setLngLat(e.lngLat)
        .setHTML(form)
        .addTo(map);
      popupElem.getElement().addEventListener("submit", (e) => {
        e.preventDefault();
        var val = document.getElementById("name");
        var des = document.getElementById("description");
        var name_value = val.value;
        var des_value = des.value;
        let c = data1.data2[0].features[0].properties;
        //  c.push(name_value)
        data1.data2.push(name_value);
        let d = data1.data2[0].features[0];
        //data1.data2[0].features[0].properties.push(name_value);
        console.log("this is come from line nu-126", d);
        // d.push({properties:})
        console.log(d);
        console.log(data1.data2[0].features[0].properties, "from console");
        console.log("hello");
      });
    });
  }
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
/* .h-full {
     height: 100%;
   }
   .w-full {
     width: 85%;
   } */
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
  height: 170px;
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
  border: 2px solid pink;
}
input {
  /* pa */
}
</style>
