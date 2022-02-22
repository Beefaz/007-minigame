<template>
  <div id="#app" class="background">
    <GameContainer/>
  </div>
</template>

<script>
import GameContainer from "@/components/GameContainer";

export default {
  name: 'App',
  components: {
    GameContainer,
  },
}
</script>

<style lang="scss" scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.background {
  width: 100vw;
  height: 100vh;
  background-color: #000000;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
