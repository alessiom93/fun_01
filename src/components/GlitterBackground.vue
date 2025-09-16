<template>
  <div ref="container" class="particles-container"></div>
</template>

<script>
import { ref, onMounted, onBeforeUnmount } from "vue";
import { tsParticles } from "tsparticles";

export default {
  name: "GlitterBackground",
  setup() {
    const container = ref(null);
    let particlesInstance = null;

    onMounted(async () => {
      particlesInstance = await tsParticles.load(container.value, {
        fullScreen: { enable: true, zIndex: 0 },
        particles: {
          number: { value: 50 },
          shape: { type: "circle" },
          size: { value: { min: 1, max: 3 } },
          move: { enable: true, speed: 0.3, direction: "bottom", outModes: "out" },
          color: { value: ["#FFD700", "#FF69B4", "#FFB6C1"] },
          opacity: { value: { min: 0.5, max: 1 } }
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
