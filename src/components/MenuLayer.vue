<template>
  <div class="menu-layer"
       :class="status"
  />
</template>

<script>
export default {
  name: 'MenuLayer',
  props: {
    status: {
      default: '',
      type: String,
    }
  },
}
</script>

<style lang="scss" scoped>
.menu-layer {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  right: 0;
  background-color: rgba(100, 100, 100, 0.5);
}
</style>
