<!--
 * @Author: shencanlong
 * @Date: 2022-07-21 22:48:14
 * @LastEditors: git config user.name && git config user.email
 * @LastEditTime: 2022-07-21 23:31:27
 * @FilePath: \learn-gaode-map\src\components\map\alarme\MapFlicker.vue
 * @Description: 
 * 
-->
<template>
  <div class="map-flicker">
    <div class="flicker"></div>
    <audio autoplay loop>
      <source src="./alarme.mp3" type="audio/mpeg" />
    </audio>
  </div>
</template>
<script>
export default {
  name: "MapFlicker",
  data() {
    return {};
  },
};
</script>
<style lang="scss" scoped>
/* 自定义样式
---------------------------------------------------------------- */
.flicker {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;

  animation-duration: 1s;
  animation-name: flicker_animation;
  animation-iteration-count: infinite;
  animation-direction: alternate;

  z-index: 999;
}

@keyframes flicker_animation {
  from {
    box-shadow: inset 0 0 4em 0.5em red;
  }

  to {
    box-shadow: inset 0 0 4em 0.5em rgba(0, 0, 0, 0);
  }
}
</style>