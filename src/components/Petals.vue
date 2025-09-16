<template>
  <div class="petals">
    <div
      v-for="p in petals"
      :key="p.id"
      class="petal"
      :style="{
        left: p.x + 'px',
        animationDuration: p.duration + 's',
        animationDelay: p.delay + 's',
        transform: 'scale(' + p.scale + ') rotate(' + p.rotate + 'deg)'
      }"
    ></div>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  name: "Petals",
  setup() {
    const petals = ref([]);

    const createPetals = () => {
      for (let i = 0; i < 25; i++) {
        petals.value.push({
          id: i,
          x: Math.random() * window.innerWidth,
          duration: 8 + Math.random() * 5,
          delay: Math.random() * 5,
          scale: 0.6 + Math.random() * 0.8,
          rotate: Math.random() * 360
        });
      }
    };

    onMounted(() => {
      createPetals();
    });

    return { petals };
  }
};
</script>

<style scoped>
.petals {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  overflow: hidden;
  z-index: -1;
}

.petal {
  position: absolute;
  top: -50px;
  width: 25px;
  height: 25px;
  background: pink;
  clip-path: ellipse(40% 60% at 50% 50%);
  opacity: 0.9;
  animation: fall linear infinite;
}

@keyframes fall {
  0% {
    transform: translateY(0) rotate(0deg);
    opacity: 1;
  }
  100% {
    transform: translateY(110vh) rotate(360deg);
    opacity: 0;
  }
}
</style>
