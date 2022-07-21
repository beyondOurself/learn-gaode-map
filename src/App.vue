<!--
 * @Author: shencanlong
 * @Date: 2022-06-26 23:12:11
 * @LastEditors: git config user.name && git config user.email
 * @LastEditTime: 2022-07-21 23:31:50
 * @FilePath: \learn-gaode-map\src\App.vue
 * @Description: 
 * 
-->

<template>
  <div id="app">
    <!-- <map-gao-de /> -->
    <!-- <MapRipple /> -->
    <!-- <MapSelect /> -->
    <Flicker />
  </div>
</template>


<script>
// import  MapGaoDe from '@/components/map/MapGaoDe.vue'
// import MapRipple from "@/components/map/MapRipple.vue";
// import MapSelect from "@/components/map/MapSelect.vue";
import Flicker from "@/components/map/alarme/flicker.vue";
export default {
  name: "App",
  components: {
    // MapGaoDe
    // MapRipple,
    // MapSelect,
    Flicker,
  },
};
</script>
