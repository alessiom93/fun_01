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
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  /* sfumatura verticale romantica */
  background: linear-gradient(to bottom,
    #6a0572 0%,    /* viola intenso */
    #ab2f74 40%,   /* fucsia caldo */
    #ff6f91 70%,   /* rosa acceso */
    #ff9671 100%   /* arancio rosato */
  );
  z-index: 0;
  pointer-events: none;
}


@keyframes rainbowFlow {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}
</style>





