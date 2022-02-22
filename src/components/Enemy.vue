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
        this.enemyFireDelay = this.randomIntFromInterval(1000, 3000);
        this.reactionTimer();
      }
    },
    enemyIsHit: function (enemyIsHit) {
      if (enemyIsHit) {
        this.$emit('reaction-time', this.reactionTime);
        this.$emit('status-change', 'game-over');
      }
    },
  },
  methods: {
    shoot(event) {
      this.enemyIsHit = true;
      const {layerX, layerY, target} = event;
      this.bloodCoordinates = {
        x: layerX - target.width / 2 + 6,
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
        this.$emit('status-change', 'died');
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
    width: calc(#{$enemy-width} / 104 * 350);
    height: $enemy-height;
    clip-path: polygon(37% 27%, 36% 22%, 39% 15%, 41% 6%, 46% 10%, 46% 5%, 49% 1%, 55% 2%, 58% 6%, 57% 12%, 55% 15%, 61% 17%, 65% 25%, 80% 23%, 82% 20%, 88% 20%, 88% 24%, 85% 24%, 82% 27%, 79% 28%, 81% 30%, 63% 32%, 64% 55%, 72% 69%, 66% 93%, 70% 97%, 69% 100%, 58% 96%, 62% 73%, 56% 65%, 50% 58%, 40% 74%, 18% 100%, 12% 100%, 12% 93%, 23% 77%, 29% 73%, 35% 59%, 32% 58%, 42% 39%, 40% 29%);
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
