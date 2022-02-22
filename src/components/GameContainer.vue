<template>
  <div class="game-container"
       :class="gameStatus === 'playing' ? 'menu-closed' : 'menu-open'"
       @mousemove="getMouseCoordinates($event)"
  >
    <Enemy
        @status-change="(val)=>gameStatus=val"
        @reaction-time="(val)=>reactionTime=val"
    />
    <Barrel :mouse-coordinates="mouseCoordinates"/>
    <MenuLayer
        v-if="gameStatus !== 'playing'"
        :game-status="gameStatus"
        :time="reactionTime"
        @status-change="(val)=>gameStatus=val"
    />
  </div>
</template>

<script>
import Enemy from "@/components/Enemy";
import Barrel from "@/components/Barrel";
import MenuLayer from "@/components/MenuLayer";

export default {
  name: 'GameContainer',
  components: {
    Enemy,
    Barrel,
    MenuLayer,
  },
  data() {
    return {
      gameStatus: 'new',
      reactionTime: 0,
      menuIsOpen: false,
      mouseCoordinates: {
        x: 0,
        y: 0,
      },
      containerCoordinates: {
        x: 0,
        y: 0,
      }
    };
  },
  updated() {
    this.setContainerCoordinates();
  },
  methods: {
    doSomething(status) {
      console.log(status);
    },
    getMouseCoordinates(event) {
      if (this.menuIsOpen) return;
      const {x, y} = event;
      this.mouseCoordinates = {
        x: x - this.containerCoordinates.x,
        y: y - this.containerCoordinates.y,
      };
    },
    setContainerCoordinates() {
      const {offsetLeft, offsetTop} = this.$el;
      this.containerCoordinates = {x: offsetLeft, y: offsetTop};
    }
  },
}
</script>
<style lang="scss" scoped>
.game-container {
  width: 400px;
  height: 400px;
  position: relative;
  overflow: hidden;
  background: #FFFFFF;

  * {
    box-sizing: border-box;
  }

  &.menu {
    &-open {
      cursor: inherit;
    }

    &-closed {
      cursor: crosshair;
    }
  }
}
</style>
