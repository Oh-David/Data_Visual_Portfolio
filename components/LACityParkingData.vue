<template>
  <div>

  </div>
</template>

<script>
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