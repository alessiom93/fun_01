<template>
  <div class="glitter-container">
    <div v-for="g in glitters" :key="g.id" class="glitter"
         :class="{ bright: g.bright }"
         :style="{
           left: g.x+'px',
           top: g.y+'px',
           width: g.size+'px',
           height: g.size+'px',
           animationDuration: g.duration+'s',
           animationDelay: g.delay+'s',
           background: g.color,
           '--move-x': g.moveX+'px',
           '--move-y': g.moveY+'px',
           opacity: g.baseOpacity
         }"></div>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  name: "Glitter",
  setup() {
    const glitters = ref([]);
    const colors = ['#ffffff', '#fffacd', '#ffe4b5', '#f0fff0'];
    let id = 0;

    onMounted(() => {
      const levels = [
        { count: 20, size: [3,5], speed: [1.5,2.5], opacity: 1 }, // vicine
        { count: 20, size: [2,3], speed: [1,1.5], opacity: 0.7 }, // medie
        { count: 20, size: [1,2], speed: [0.5,1], opacity: 0.5 }  // lontane
      ];

      levels.forEach(level => {
        for (let i = 0; i < level.count; i++) {
          glitters.value.push({
            id: id++,
            x: Math.random() * window.innerWidth,
            y: Math.random() * window.innerHeight,
            size: level.size[0] + Math.random() * (level.size[1]-level.size[0]),
            duration: level.speed[0] + Math.random() * (level.speed[1]-level.speed[0]),
            delay: Math.random() * 5,
            color: colors[Math.floor(Math.random() * colors.length)],
            moveX: Math.random() * 4 - 2,
            moveY: Math.random() * 4 - 2,
            bright: Math.random() < 0.3,
            baseOpacity: level.opacity
          });
        }
      });
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
  border-radius: 50%;
  filter: drop-shadow(0 0 3px rgba(255,255,255,0.5)) drop-shadow(0 0 5px rgba(255,255,255,0.3));
  animation: sparkle infinite ease-in-out;
  will-change: transform, opacity, box-shadow;
}

/* stelle bright con scia */
.glitter.bright {
  filter: drop-shadow(0 0 6px rgba(255,255,255,0.9)) drop-shadow(0 0 10px rgba(255,255,255,0.7));
  animation: sparkle-bright infinite ease-in-out;
}

/* sparkle normale */
@keyframes sparkle {
  0%,100% { opacity:0; transform: scale(1) translate(0,0); }
  25% { opacity:0.5; transform: scale(1.2) translate(var(--move-x), var(--move-y)); }
  50% { opacity:1; transform: scale(1.5) translate(calc(var(--move-x) * -1), calc(var(--move-y) * -1)); }
  75% { opacity:0.5; transform: scale(1.2) translate(calc(var(--move-x)/2), calc(var(--move-y)/2)); }
}

/* sparkle con scia per stelle bright */
@keyframes sparkle-bright {
  0%,100% { 
    opacity:0; 
    transform: scale(1) translate(0,0); 
    box-shadow: 0 0 0px rgba(255,255,255,0);
  }
  25% { 
    opacity:0.5; 
    transform: scale(1.3) translate(var(--move-x), var(--move-y)); 
    box-shadow: 0 0 6px rgba(255,255,255,0.6), 0 0 10px rgba(255,255,255,0.4);
  }
  50% { 
    opacity:1; 
    transform: scale(1.6) translate(calc(var(--move-x) * -1), calc(var(--move-y) * -1)); 
    box-shadow: 0 0 10px rgba(255,255,255,0.8), 0 0 15px rgba(255,255,255,0.6);
  }
  75% { 
    opacity:0.5; 
    transform: scale(1.3) translate(calc(var(--move-x)/2), calc(var(--move-y)/2)); 
    box-shadow: 0 0 6px rgba(255,255,255,0.6), 0 0 10px rgba(255,255,255,0.4);
  }
}
</style>
