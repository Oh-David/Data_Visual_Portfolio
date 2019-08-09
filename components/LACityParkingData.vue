<template>
  <div>

  </div>
</template>

<script>
// Documentation for API
// https://data.lacity.org/A-Safe-City/Arrest-Data-from-2010-to-Present/yru6-6re4

import axios from 'axios'
import * as d3 from 'd3'

import { eventBus } from '~/plugins/eventBus.js'

  export default {
    name: "LA-City-Parking-Citation",
    mounted() {
      var parseTime = d3.timeParse("%Y-%m-%dT%H:%M:%S.%L");

      axios.get(`https://data.lacity.org/resource/yru6-6re4.json`)
        .then(res => {

        var data = res.data.map(d => {
          return {
            age: d.age,
            area_desc: d.area_desc,
            arst_date: parseTime(d.arst_date),
            time: d.time,
            arst_typ_cd: d.arst_typ_cd,
            charge: d.charge,
            chrg_desc: d.chrg_desc,
            chrg_grp_cd: d.chrg_grp_cd,
            descent_cd: d.descent_cd,
            crsst: d.crsst,
            sex_cd: d.sex_cd,
            grp_description: d.grp_description
          }
        })
        eventBus.$emit('LA_CrimeData', data);
      })
    }
  }
</script>

<style>
  
</style>

        var months = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"]
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
        console.log(countSex);
        console.log(countTime);