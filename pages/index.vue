<template>
  <div>
    <LACityParkingData />
    <div class="container">
      <div id="crimeheatmap-container">
        <CrimeHeatMap />
      </div>
      <div id="barchart-container">
        <BarChart />
      </div>
    </div>
  </div>
</template>

<script>
// import * as Vue2Leaflet from 'vue2-leaflet';
// var { LMap, LTileLayer, LMarker } = Vue2Leaflet;

import Logo from '~/components/Logo.vue'
import BarChart from '~/components/BarChart.vue'
import CrimeHeatMap from '~/components/CrimeHeatMap.vue'
import LACityParkingData from '~/components/LACityParkingData.vue'

export default {
  components: {
    BarChart,
    CrimeHeatMap,
    LACityParkingData,
    // LMap,
    // LTileLayer,
    // LMarker,
  },
  // data() {
  //   return {
  //     zoom: 20,
  //     center: L.latLng(34.0522, 118.2437),
  //     url:'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
  //     attribution:'&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
  //     marker: L.latLng(34.0522, 118.2437)
  //   }
  // },
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}

#crimeheatmap-container {
  height: 600px;
  height: 600px;
}
</style>
