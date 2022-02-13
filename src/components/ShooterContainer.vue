<template>
  <div class="shooter-container"
       :class="menuIsOpen ? 'menu-open' : 'menu-closed'"
       @mousemove="getMouseCoordinates($event)"
  >
    <Enemy/>
    <Barrel :mouse-coordinates="mouseCoordinates"/>
    <MenuLayer v-if="menuIsOpen"/>
  </div>
</template>

<script>
import Barrel from "./Barrel";
import Enemy from "./Enemy";
import MenuLayer from "@/components/MenuLayer";

export default {
  name: 'ShooterContainer',
  components: {
    MenuLayer,
    Barrel,
    Enemy
  },
  data() {
    return {
      menuIsOpen: false,
      mouseCoordinates: {
        x: 0,
        y: 0,
      }
    };
  },
  methods: {
    getMouseCoordinates(event) {
      if (this.menuIsOpen) return;
      const {x, y} = event;
      this.mouseCoordinates = {x, y};
    },
  }
}
</script>
<style lang="scss" scoped>
.shooter-container {
  width: 400px;
  height: 400px;
  border: 1px solid red;
  position: relative;
  overflow: hidden;
  background: #FFFFFF;

  &.menu {
    &-open {
      cursor: inherit;
    }

    &-closed {
      cursor: none;
    }
  }
}
</style>
