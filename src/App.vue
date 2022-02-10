<template>
  <div id="app">
    <Background/>
    <BarrelLayer/>
  </div>
</template>

<script>
import Background from "../../007-minigame/src/components/BarrelLayer";
import BarrelLayer from "../../007-minigame/src/components/BarrelLayer";

export default {
  name: 'App',
  components: {
    BarrelLayer,
    Background
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
