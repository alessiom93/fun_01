<template>
  <div class="hearts-fall">
    <div v-for="h in hearts" :key="h.id" class="fall-heart"
         :style="{
           left: h.x + 'px',
           top: h.top + 'px',
           '--float-offset': h.floatOffset + 'px',
           '--pulse-scale': h.pulseScale,
           '--rotate-speed': h.rotateSpeed + 'deg',
           transform: 'scale(' + h.scale + ') rotate(' + h.rotateStart + 'deg)',
           animation: 'float 4s infinite ease-in-out, pulse 2s infinite ease-in-out, rotate ' + h.rotateDuration + 's linear infinite'
         }">
      <svg viewBox="0 0 32 29.6" xmlns="http://www.w3.org/2000/svg">
        <path d="M23.6,0c-3.4,0-6.4,2.1-7.6,5.1C14.8,2.1,11.8,0,8.4,0C3.8,0,0,3.8,0,8.4
                 c0,9.3,16,21.2,16,21.2s16-11.9,16-21.2C32,3.8,28.2,0,23.6,0z" :fill="h.color"/>
      </svg>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  name: "FallingHearts",
  setup() {
    const hearts = ref([]);
    const colors = ["#ff1493","#ff4d6d","#ff69b4","#ff85c1","#c71585","#e75480"];
    let heartId = 0;
    const MAX_HEARTS = 30; // massimo numero di cuori

    function createHeart() {
      if (hearts.value.length >= MAX_HEARTS) return;
      const heart = {
        id: heartId++,
        x: Math.random() * window.innerWidth,
        top: -50,
        scale: 0.5 + Math.random() * 0.8,
        color: colors[Math.floor(Math.random() * colors.length)],
        rotateStart: Math.random() * 360,
        rotateSpeed: Math.random() * 60 - 30,
        rotateDuration: 3 + Math.random() * 2,
        floatOffset: Math.random() * 30 - 15,
        pulseScale: 0.9 + Math.random() * 0.3,
        speed: 1 + Math.random() * 1.5,
      };
      hearts.value.push(heart);
    }

    function animateHearts() {
      hearts.value.forEach(h => {
        h.top += h.speed;
        if (h.top > window.innerHeight) h.top = -50;
      });
      requestAnimationFrame(animateHearts);
    }

    onMounted(() => {
      // generiamo inizialmente qualche cuore
      for (let i = 0; i < 15; i++) createHeart();

      // generazione graduale
      setInterval(() => createHeart(), 500);

      animateHearts();
    });

    return { hearts };
  }
};
</script>

<style scoped>
.hearts-fall {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 1;
}
.fall-heart {
  position: absolute;
  width: 32px;
  height: 30px;
}
.fall-heart svg {
  width: 100%;
  height: 100%;
  filter:
    drop-shadow(0 0 4px rgba(255, 105, 180, 0.8))
    drop-shadow(0 0 8px rgba(255, 105, 180, 0.6))
    drop-shadow(0 0 12px rgba(255, 105, 180, 0.4));
  transform-origin: center center;
  will-change: transform;
}

/* oscillazione orizzontale */
@keyframes float {
  0%, 100% { transform: translateX(0px); }
  25% { transform: translateX(var(--float-offset)); }
  50% { transform: translateX(calc(var(--float-offset) * -1)); }
  75% { transform: translateX(calc(var(--float-offset)/2)); }
}

/* pulsazione */
@keyframes pulse {
  0%,100% { transform: scale(1); }
  50% { transform: scale(var(--pulse-scale)); }
}

/* rotazione */
@keyframes rotate {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(var(--rotate-speed)); }
}
</style>
