<template>
  <div class="menu-layer"
       :class="gameStatus"
  >
    <div class="tutorial">
      <img
          src="../assets/images/svg/question-mark.svg"
          alt=""
      >
    </div>
    <div v-if="gameResult.reactionTime !== 0 && gameResult.enemyIsHit">
      Nice shot! Your reaction time was {{ gameResult.reactionTime }}ms
    </div>
    <div v-if="!gameResult.enemyIsHit && gameResult.reactionTime === null">
      Ouch! You died
    </div>
    <button v-if="status !== 'playing'"
            @click="status = 'playing'">
      Start new game
    </button>
    <div v-if="bestScore">
      Your best time: {{ bestScore }}
    </div>
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
    gameResult: {
      reactionTime: {
        type: Number,
        default: 0,
      },
      enemyIsHit: {
        type: Boolean,
        default: false
      },
    },
  },
  data() {
    return {
      status: 'new',
      bestScore: null,
    }
  },
  mounted() {
    this.handleCookieData();
  },
  methods: {
    handleCookieData() {
      if (localStorage.getItem('best-score')) this.bestScore = JSON.parse(localStorage.getItem('best-score'));
      if (this.gameResult.enemyIsHit && (this.bestScore > this.gameResult.reactionTime || !this.bestScore)) {
        localStorage.removeItem('best-score');
        localStorage.setItem('best-score', JSON.stringify(this.gameResult.reactionTime));
      }
    },
  },
  watch: {
    status: function (val) {
      this.$emit('status-change', val)
    },
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
  background-color: rgba(100, 100, 100, 0.7);
  border: 2px solid #FFFFFF;
  color: #FFFFFF;
  font-family: bond, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  text-transform: uppercase;
  letter-spacing: 1px;

  & * + * {
    margin: 10px 0;
  }

  .tutorial {
    position: absolute;
    right: 1%;
    top: 1%;

    &:hover::after {
      position: absolute;
      content: 'As true 007, you will only shoot in retaliation, when enemy aims his gun at you';
      top: 50%;
      right: 50%;
      color: white;
      font-size: 12px;
    }

    img {
      position: relative;
    }
  }

  button {
    background-color: #FFFFFF;
    padding: 10px;
    border-radius: 25px;
    font-family: bond, sans-serif;
    text-transform: uppercase;
    letter-spacing: 1px;
  }
}
</style>
