<template>
  <div class="game-container"
       :class="gameStatus === 'playing' ? 'menu-closed' : 'menu-open'"
       @mousemove="getMouseCoordinates($event)"
  >
    <template v-if="gameStatus!==null">
      <Enemy
          :key="gameSeed"
          @game-result="(val) => gameResult = val"
          @game-status="(val) => gameStatus = val"
      />
    </template>
    <Barrel :mouse-coordinates="mouseCoordinates"/>
    <MenuLayer
        v-if="gameStatus !== 'playing' || gameStatus === null"
        :game-result="gameResult"
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
      gameSeed: 0,
      gameStatus: null,
      gameResult: {},
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
    getMouseCoordinates(event) {
      if (this.gameStatus !== 'playing') return;
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
  watch: {
    gameStatus: function (val) {
      if (val === 'playing') {
        this.gameSeed++;
      }
    },
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
