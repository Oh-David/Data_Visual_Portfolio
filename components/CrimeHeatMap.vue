<template>
  <div style="height: 100%; width: 100%">
    <link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.7.3/leaflet.css" />

    <div class="info" style="height: 15%">
      <span>Center: {{ center }}</span>
      <span>Zoom: {{ zoom }}</span>
      <span>Bounds: {{ bounds }}</span>
    </div>
    <l-map
      style="height: 80%; width: 100%"
      :zoom="zoom"
      :center="center"
      @update:zoom="zoomUpdated"
      @update:center="centerUpdated"
      @update:bounds="boundsUpdated"
    >
      <l-tile-layer :url="url"></l-tile-layer>
    </l-map>
  </div>
</template>

<script>
import * as d3 from 'd3';
import alasql from 'alasql';

import * as Vue2Leaflet from 'vue2-leaflet';
var { LMap, LTileLayer, LMarker } = Vue2Leaflet;

import { eventBus } from '~/plugins/eventBus.js';
import LACityParkingData from '~/components/LACityParkingData.vue';

  export default {
    name: "Bar-Chart",
    components: {
      LACityParkingData,
      LMap,
      LTileLayer,
      LMarker,
    },
    data() {
      return {
        zoom: 5,
        center: [34.0522, 118.2437],
        marker: [34.0522, 118.2437],
        bounds: null,
        url:'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
        attribution:'&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      }
    },
    methods: {
      zoomUpdated (zoom) {
        this.zoom = zoom;
      },
      centerUpdated (center) {
        this.center = center;
      },
      boundsUpdated (bounds) {
        this.bounds = bounds;
      }
    },
    mounted() {
      var margin = {top: 50, right: 50, bottom: 50, left: 50};
      var width = 600 - margin.left - margin.right;
      var height = 600 - margin.top - margin.bottom;

      var svg = d3.select("#mapid").append('svg')
        .attr("width", width + margin.left + margin.right)
        .attr("height", height + margin.top + margin.bottom)
        .append("g")
        .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

      var x = d3.scaleBand().range([0, width]);
      var y = d3.scaleLinear().range([height, 0]);

      eventBus.$on('LA_CrimeData', data => {
        console.log(data);

      });
    }
  }
</script>

<style>

</style>