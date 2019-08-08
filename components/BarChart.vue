<template>
  <div>
    <h1>Bar Chart</h1>
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
    // methods: {
    //   makeVis(crimeData) {
    //     var margin = { top: 30, right: 50, bottom: 30, left: 50 },
    //         width  = 550 - margin.left - margin.right,
    //         height = 250 - margin.top  - margin.bottom;
        
    //     var xScale = d3.scale.ordinal()
    //         .domain(months)
    //         .rangeRoundBands([0, width], 0.1);

    //     var yScale = d3.scale.linear()
    //         .range([height, 0]);
        
    //     var canvas = d3.select("#vis-container")
    //         .append("svg")
    //           .attr("width",  width  + margin.left + margin.right)
    //           .attr("height", height + margin.top  + margin.bottom)
    //         .append("g")
    //           .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

    //     var yAxis = d3.svg.axis()
    //         .scale(yScale)
    //         .orient("left");

    //     var yAxisHandleForUpdate = canvas.append("g")
    //         .attr("class", "y axis")
    //         .call(yAxis);

    //     yAxisHandleForUpdate.append("text")
    //         .attr("transform", "rotate(-90)")
    //         .attr("y", 6)
    //         .attr("dy", ".71em")
    //         .style("text-anchor", "end")
    //         .text("Value");

    //     var updateBars = function(data) {
    //         // First update the y-axis domain to match data
    //         yScale.domain( d3.extent(data) );
    //         yAxisHandleForUpdate.call(yAxis);

    //     var bars = canvas.selectAll(".bar").data(data);

    //     // Add bars for new data
    //     bars.enter()
    //         .append("rect")
    //         .attr("class", "bar")
    //         .attr("x", function(d,i) { return xScale( months[i] ); })
    //         .attr("width", xScale.rangeBand())
    //         .attr("y", function(d,i) { return yScale(d); })
    //         .attr("height", function(d,i) { return height - yScale(d); });

    //     // Update old ones, already have x / width from before
    //     bars
    //         .transition().duration(250)
    //         .attr("y", function(d,i) { return yScale(d); })
    //         .attr("height", function(d,i) { return height - yScale(d); });

    //     // Remove old ones
    //     bars.exit().remove();

    //     // Get names of cereals, for dropdown
    //     var cereals = Object.keys(crimeData).sort();

    //     var dropdown = d3.select("#vis-container")
    //         .insert("select", "svg")
    //         .on("change", dropdownChange);

    //     dropdown.selectAll("option")
    //         .data(cereals)
    //         .enter().append("option")
    //         .attr("value", function (d) { return d; })
    //         .text(function (d) {
    //             return d[0].toUpperCase() + d.slice(1,d.length); // capitalize 1st letter
    //         });

    //     var initialData = crimeData[ crime[0] ];
    //     updateBars(initialData);
    //     };
    //   },
    // },
    mounted() {
      eventBus.$on('LA_CrimeData', data => {
        var crimeType = {};
        var months = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"]
        console.log(data);

        // Counts number of arrests by age.
        var countAges = alasql('SELECT INDEX age, COUNT(*) AS cnt FROM ? GROUP BY age', [data]);
        // Counts area where arrest was made
        var countArea = alasql('SELECT INDEX area_desc, COUNT(*) AS cnt FROM ? GROUP BY area_desc', [data]);
        // Counts type of crime ie, dependant (d), felony (f), infraction (i), misdemeanor (m), other (o)
        var countCrimeType = alasql('SELECT INDEX arst_typ_cd, COUNT(*) AS cnt FROM ? GROUP BY arst_typ_cd', [data]);

        // counts Crime Description
        var countChargeDesc = alasql('SELECT INDEX chrg_desc, COUNT(*) AS cnt FROM ? GROUP BY chrg_desc', [data]);

        // Counts race ethnicity, origin
        var countDemographics = alasql('SELECT INDEX descent_cd, COUNT(*) AS cnt FROM ? GROUP BY descent_cd', [data]);
        
        // Counts Crime by Category
        var countCrimeByGroupDesc = alasql('SELECT INDEX grp_description, COUNT(*) AS cnt FROM ? GROUP BY grp_description', [data]);
        
        // Counts Sex (Male / Female)
        var countSex = alasql('SELECT INDEX sex_cd, COUNT(*) AS cnt FROM ? GROUP BY sex_cd', [data]);
        
        // Counts time of arrest, **Look into grouping by category
        var countTime = alasql('SELECT INDEX time, COUNT(*) AS cnt FROM ? GROUP BY time', [data]);

        console.log(countAges);
        console.log(countArea);
        console.log(countCrimeType);
        console.log(countChargeDesc);
        console.log(countDemographics);
        console.log(countGroupDesc);
        console.log(countSex);
        console.log(countTime);

        var crimeData = data.forEach(function(d) {
          var crime = d.chrg_desc;
          crimeType[crime] = [];

          months.forEach(function(month) {
            crimeType[crime].push( +[month])
          });
        });
      })
    }
  }
</script>

<style>
  
</style>

const element = document.getElementById('#bar-chart');
        const margin = {top: 20, right: 10, bottom: 20, left: 10};
        const width = 960 - margin.left - margin.right,
        height = 500 - margin.top - margin.bottom;

        var svg = d3.select("bar-chart").append("svg")
          .attr("width", width + margin.left + margin.right)
          .attr("height", height + margin.top + margin.bottom)
        .append("g")
          .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

        var xScale = d3.scaleLinear().range([])