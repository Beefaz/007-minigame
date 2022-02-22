<template>
  <div class="menu-layer"
       :class="status">
    <div v-if="time !== 0">
      Congratulations! Your time was {{ time }}ms
    </div>
    <div v-if="status === 'died'">
      You died
    </div>
    <button v-if="status==='new'"
            @click="status = 'playing'">
      Start new game
    </button>
    <button v-if="status==='game-over' || status === 'died'">
      Restart
    </button>
  </div>
</template>

<script>
export default {
  name: 'MenuLayer',
  props: {
    gameStatus: {
      type: String,
      default: 'new',
    },
    time: {
      type: Number,
      default: 0,
    }
  },
  data(){
    return {
      status: 'new',
    }
  },
  watch: {
    status: function (val){
      this.$emit('status-change', val)
    }
  }
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
  border: 2px inset white;
  color: white;

  &.new {
    background-color: #000000;
  }

  &.playing {
    background-color: rgba(100, 100, 100, 0);
  }

  &.game-over {
    background-color: rgba(100, 100, 100, 1);
  }
}
</style>
