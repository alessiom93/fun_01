<template>
  <div ref="container" class="particles-container"></div>
</template>

<script>
import { ref, onMounted, onBeforeUnmount } from "vue";
import { tsParticles } from "tsparticles";

export default {
  name: "HeartParticles",
  setup() {
    const container = ref(null);
    let particlesInstance = null;

    onMounted(async () => {
      particlesInstance = await tsParticles.load(container.value, {
        fullScreen: { enable: true },
        particles: {
          number: { value: 30 },
          shape: { type: "heart" },
          size: { value: { min: 10, max: 20 } },
          move: { enable: true, speed: 1, direction: "bottom", outModes: "out" },
          color: { value: ["#FF69B4", "#FF1493", "#FFB6C1"] },
          opacity: { value: 0.8 }
        },
        detectRetina: true
      });
    });

    onBeforeUnmount(() => {
      if (particlesInstance) particlesInstance.destroy();
    });

    return { container };
  }
};
</script>

<style scoped>
.particles-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
}
</style>
