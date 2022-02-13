<template>
  <div id="#app" class="background">
    <ShooterContainer/>
  </div>
</template>

<script>
import ShooterContainer from "@/components/ShooterContainer";

export default {
  name: 'App',
  components: {
    ShooterContainer
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
}
</style>
