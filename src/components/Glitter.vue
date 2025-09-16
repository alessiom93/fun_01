<template>
  <div class="glitter-container">
    <div v-for="g in glitters" :key="g.id" class="glitter"
         :style="{ left: g.x+'px', top: g.y+'px', animationDuration: g.duration+'s' }"></div>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  name: "Glitter",
  setup() {
    const glitters = ref([]);
    onMounted(() => {
      for (let i=0;i<60;i++){
        glitters.value.push({
          id:i,
          x: Math.random()*window.innerWidth,
          y: Math.random()*window.innerHeight,
          duration: 2 + Math.random()*3
        });
      }
    });
    return { glitters };
  }
};
</script>

<style scoped>
.glitter-container {
  position: fixed;
  top:0; left:0;
  width:100%; height:100%;
  pointer-events: none;
  z-index: 1;
}
.glitter {
  position: absolute;
  width: 4px; height: 4px;
  border-radius: 50%;
  background: white;
  opacity: 0;
  animation: sparkle infinite;
}
@keyframes sparkle {
  0%,100%{opacity:0;}
  50%{opacity:1; transform:scale(1.5);}
}
</style>
