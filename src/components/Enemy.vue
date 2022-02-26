<template>
  <div
      class="hitbox"
      :class="`${enemyIsAiming ? 'aiming' : ''}`"
      :style="`animation-delay: -${animationDelay}s;`"
  >
    <img
        v-if="!enemyIsAiming"
        class="hitbox--image"
        src="../assets/images/png/manStanding.png"
        alt=""
        @dragstart.prevent
    >
    <template v-if="enemyIsAiming">
      <img
          class="hitbox--image"
          src="../assets/images/png/manAiming.png"
          alt=""
          @dragstart.prevent
      >
      <div
          class="hitbox--box"
          @click="shoot($event)"
      >
      </div>
      <img
          v-if="enemyIsHit"
          class="blood"
          :style="`
           left:${bloodCoordinates.x}px;
           top:${bloodCoordinates.y}px;
           `"
          src="../assets/images/svg/blood.svg"
          alt=""
      >
    </template>
  </div>
</template>

<script>
export default {
  name: 'Enemy',
  data() {
    return {
      enemyIsAiming: false,
      enemyIsHit: false,
      enemyFireDelay: false,
      animationDelay: 0,
      reactionTime: 0,
      gameResult: {
        enemyIsHit: false,
        reactionTime: null
      },
      bloodCoordinates: {
        x: 0,
        y: 0,
      }
    }
  },
  created() {
    this.animationDelay = this.randomIntFromInterval(3000, 100000);
  },
  mounted() {
    this.enemyWalkTimer();
  },
  watch: {
    enemyIsAiming: function (enemyIsAiming) {
      if (enemyIsAiming) {
        this.enemyFireDelay = this.randomIntFromInterval(300, 3000);
        this.reactionTimer();
      }
    },
    enemyIsHit: function (enemyIsHit) {
      if (enemyIsHit) {
        this.gameResult = {
          enemyIsHit: this.enemyIsHit,
          reactionTime: this.reactionTime,
        }
      }
    },
  },
  methods: {
    shoot(event) {
      if (this.enemyIsHit) {
        return;
      }
      this.enemyIsHit = true;
      const {layerX, layerY} = event;
      this.bloodCoordinates = {
        x: layerX - 35,
        y: layerY - 10,
      }
    },
    randomIntFromInterval(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
    enemyWalkTimer() {
      setTimeout(() => {
        this.enemyIsAiming = true;
      }, this.randomIntFromInterval(3000, 5000));
    },
    reactionTimer() {
      if (!this.enemyIsHit && this.reactionTime <= this.enemyFireDelay) {
        this.reactionTime = this.reactionTime + 7;
        setTimeout(() => this.reactionTimer(), 7);
      } else {
        this.$emit('game-result', this.gameResult);
        this.$emit('game-status', 'game-over');
      }
    },
  },
}
</script>

<style lang="scss" scoped>
$enemy-height: 104px;
$enemy-width: calc(#{$enemy-height} / 350 * 104);

.hitbox {
  position: relative;
  width: $enemy-width;
  height: $enemy-height;
  display: flex;
  justify-content: center;
  align-items: center;

  &--image {
    height: $enemy-height;
    object-fit: cover;
    user-select: none;
  }

  &--box {
    position: absolute;
    width: calc(#{$enemy-width} / 104 * 270);
    height: $enemy-height;
    clip-path: polygon(34% 27%, 32% 23%, 37% 11%, 39% 6%, 44% 10%, 45% 5%, 47% 2%, 51% 1%, 57% 2%, 60% 6%, 60% 10%, 56% 15%, 64% 17%, 70% 25%, 85% 24%, 93% 20%, 99% 20%, 99% 24%, 96% 24%, 90% 27%, 89% 30%, 80% 31%, 67% 32%, 67% 55%, 73% 62%, 77% 69%, 71% 93%, 76% 97%, 76% 99%, 70% 99%, 61% 95%, 66% 73%, 57% 65%, 50% 58%, 36% 74%, 8% 100%, 4% 100%, 0% 97%, 1% 92%, 15% 78%, 23% 73%, 31% 59%, 27% 57%, 39% 39%, 37% 29%);
  }

  .blood {
    position: absolute;
    width: calc(#{$enemy-height} / 104 * 20);
    height: calc(#{$enemy-height} / 104 * 20);
    user-select: none;
  }

  @keyframes horizontal-movement {
    from {
      margin-left: 0;
    }
    to {
      margin-left: calc(100% - #{$enemy-width});
    }
  }

  @keyframes vertical-movement {
    from {
      margin-top: 0;
    }
    to {
      margin-top: calc(100% - #{$enemy-height});
    }
  }

  & {
    animation-name: horizontal-movement, vertical-movement;
    animation-duration: 3s, 2s;
    animation-iteration-count: infinite;
    animation-direction: alternate;
    animation-timing-function: linear;

    &.aiming {
      animation-play-state: paused;
    }
  }
}
</style>
