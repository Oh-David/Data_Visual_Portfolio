<template>
  <div>
    <LACityParkingData />
  </div>
</template>

<script>
import { eventBus } from '~/plugins/eventBus.js'
import LACityParkingData from '~/components/LACityParkingData.vue'

  export default {
    name: "Bar-Chart",
    components: {
      LACityParkingData
    },
    mounted() {
      eventBus.$on('LA_CrimeData', data => {
        console.log("Data emitted to BarChart");
        console.log(data);
      })
    }
  }
</script>

<style>
  
</style>