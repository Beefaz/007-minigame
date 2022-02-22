<template>
  <img class="barrel"
       :style="`
           left:${mouseCoordinates.x-405}px;
           top:${mouseCoordinates.y-405}px;
           `"
       src="../assets/images/png/barrel.png"
       alt=""
  >
</template>

<script>
export default {
  name: 'Barrel',
  props: ['mouseCoordinates'],
}
</script>

<style lang="scss" scoped>
.barrel {
  position: absolute;
  width: 810px;
  height: 810px;
  pointer-events: none;
}
</style>
