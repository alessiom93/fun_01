<template>
  <div id="app">
    <div class="background"></div> <!-- sfondo animato -->
    <Glitter />
    <FallingHearts />
    <MouseTrailHearts />
    <LoveMessage />
  </div>
</template>

<script>
import Glitter from './components/Glitter.vue'
import FallingHearts from './components/FallingHearts.vue'
import MouseTrailHearts from './components/MouseTrailHearts.vue'
import LoveMessage from './components/LoveMessage.vue'

export default {
  name: 'App',
  components: { Glitter, FallingHearts, MouseTrailHearts, LoveMessage }
}
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  height: 100vh;
  overflow: hidden;
  position: relative;
}

.background {
  position: fixed;
  top:0; left:0; width:100%; height:100%;
  background: linear-gradient(270deg, #ffe6f0, #ffccd5, #ffd9e8, #e6ccff, #ffcce6, #fff0f5);
  background-size: 1800% 1800%;
  animation: rainbowFlow 25s ease infinite;
  z-index: 0; /* sotto tutti gli effetti */
}

@keyframes rainbowFlow {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}
</style>
