<template>
  <div>
    <h1>Crime Histogram</h1>
    <div id="bar-chart">
      <!-- Bar Chart gets drawn here. -->
    </div>
  </div>
</template>

<script>
import * as d3 from 'd3';
import alasql from 'alasql';

import { eventBus } from '~/plugins/eventBus.js';
import LACityParkingData from '~/components/LACityParkingData.vue';

  export default {
    name: "Bar-Chart",
    components: {
      LACityParkingData
    },
    methods: {

    },
    mounted() {
      var margin = {top: 50, right: 50, bottom: 50, left: 50};
      var width = 600 - margin.left - margin.right;
      var height = 600 - margin.top - margin.bottom;

      var svg = d3.select("#bar-chart").append('svg')
        .attr("width", width + margin.left + margin.right)
        .attr("height", height + margin.top + margin.bottom)
        .append("g")
        .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

      var x = d3.scaleBand().range([0, width]);
      var y = d3.scaleLinear().range([height, 0]);

      eventBus.$on('LA_CrimeData', data => {
        console.log(data);
        // Counts Sex (Male / Female)
        var countSex = alasql('SELECT INDEX sex_cd, COUNT(*) AS cnt FROM ? GROUP BY sex_cd', [data]);
        
        console.log(countSex);

        x.domain(data.map(function(d) { return d.arst_typ_cd }));
        y.domain([0, d3.max(data, function(d) { return d.age; })]);

        // function creates bar charts using rect
        svg.selectAll(".bar")
          .data(data)
          .enter().append("rect")
          .attr("class", "bar")
          .attr("x", function(d) { return x(d.arst_typ_cd); })
          .attr("width", (x.bandwidth()/2)) // x.bandwidth()
          .attr("y", function(d) { return y(d.age); })
          .attr("height", function(d) { return height - y(d.age) ; });

        svg.append("g")
          .attr("transform", "translate(0," + height + ")")
          .call(d3.axisBottom(x));

        svg.append("g")
          .call(d3.axisLeft(y));

      });
    }
  }
</script>

<style>
  
</style>