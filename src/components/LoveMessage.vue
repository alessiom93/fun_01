<template>
  <div class="love-container">
    <h1 ref="title">Per la mia Livia ❤️</h1>
    <p ref="message">Buon anniversario di fidanzamento!</p>
    <p ref="message2">❤️ da: Alessio ❤️</p>
    <button @click="surprise">Clicca per un messaggio speciale 💌</button>

    <div ref="explosion" class="explosion-container" aria-hidden="true"></div>
    <div v-if="showPopup" class="popup">❤️❤️❤️ TI AMO ❤️❤️❤️</div>
    <audio ref="audio" src="/music/love.mp3" autoplay loop></audio>
  </div>
</template>

<script>
import { ref, onMounted, onBeforeUnmount } from "vue";
import { gsap } from "gsap";

export default {
  setup() {
    const title = ref(null);
    const message = ref(null);
    const message2 = ref(null);
    const explosion = ref(null);
    const audio = ref(null);
    const showPopup = ref(false);

    // pool e controllo intervallo
    let pool = [];
    let available = [];
    let tickInterval = null;

    const POOL_SIZE = 180; // aumentare se vuoi più sovrapposizioni senza reuse
    const TICK_MS = 40; // quanto spesso generare (40ms = fluido)
    const DURATION_MS = 10000; // 10 secondi di emissione

    onMounted(() => {
      gsap.from(title.value, { y: -50, opacity: 0, duration: 1, ease: "bounce.out" });
      gsap.from(message.value, { y: 50, opacity: 0, duration: 1.5, delay: 0.5 });
      gsap.from(message2.value, { y: 50, opacity: 0, duration: 1.5, delay: 0.6 });
      audio.value?.play().catch(()=>{});

      // crea il pool di elementi (una sola volta)
      createPool();
      // pulizia al resize per ricalcolare raggio se necessario
      window.addEventListener("resize", handleResize);
    });

    onBeforeUnmount(() => {
      clearInterval(tickInterval);
      window.removeEventListener("resize", handleResize);
    });

    function handleResize(){}
    function createPool() {
      pool = [];
      available = [];
      if (!explosion.value) return;
      // svuota container (se riavvi)
      explosion.value.innerHTML = "";

      for (let i = 0; i < POOL_SIZE; i++) {
        const el = document.createElement("div");
        el.className = "heart-explosion";
        el.setAttribute("aria-hidden", "true");
        // SVG del cuore (dimensione neutra, la scala la gestiamo con transform)
        el.innerHTML = `<svg viewBox="0 0 32 29.6" width="28" height="28" preserveAspectRatio="xMidYMid meet">
          <path d="M23.6,0c-3.4,0-6.4,2.1-7.6,5.1C14.8,2.1,11.8,0,8.4,0
            C3.8,0,0,3.8,0,8.4c0,9.3,16,21.2,16,21.2s16-11.9,16-21.2
            C32,3.8,28.2,0,23.6,0z" fill="#ff69b4"/>
        </svg>`;
        // stile iniziale: centro, invisibile
        el.style.position = "absolute";
        el.style.top = "0";
        el.style.left = "0";
        el.style.pointerEvents = "none";
        el.style.opacity = "0";
        el.style.transform = "translate3d(0,0,0) scale(1) rotate(0deg)";
        el.style.willChange = "transform, opacity";
        explosion.value.appendChild(el);
        pool.push(el);
        available.push(el);
      }
    }

    function getRandomColor() {
      const colors = ["#ff4d6d","#ffa64d","#ffff66","#66ff99","#66ffff","#6699ff","#b266ff"];
      return colors[Math.floor(Math.random() * colors.length)];
    }

function launchHeart(screenW, screenH) {
  if (available.length === 0) return; 
  const el = available.pop();

  // colore casuale
  const svgPath = el.querySelector("path");
  if (svgPath) svgPath.setAttribute("fill", getRandomColor());

  // parametri
  const angle = Math.random() * Math.PI * 2;
  const maxRadius = Math.sqrt(screenW * screenW + screenH * screenH) / 2 + 80;
  const minRadius = Math.min(screenW, screenH) / 6;
  const radius = minRadius + Math.random() * (maxRadius - minRadius);

  const x = Math.cos(angle) * radius;
  const y = Math.sin(angle) * radius;

  const scale = 0.6 + Math.random() * 0.8;
  const duration = 1.6 + Math.random() * 2.4;
  const rotateFrom = Math.random() * 360;
  const rotateTo = rotateFrom + (Math.random() * 180 - 90);

  // reset
  gsap.killTweensOf(el);
  gsap.set(el, {
    x: 0,
    y: 0,
    scale,
    rotation: rotateFrom,
    opacity: 1, // sempre visibili
    transformOrigin: "center center",
  });

  const jitterX = (Math.random() - 0.5) * 6;
  const jitterY = (Math.random() - 0.5) * 6;

  // animazione senza svanire
  gsap.to(el, {
    x: x + jitterX,
    y: y + jitterY,
    rotation: rotateTo,
    duration,
    ease: "power1.out",
    force3D: true,
onComplete: () => {
  // reset invisibile: rimesso disponibile ma non visibile al centro
  gsap.set(el, { x: 0, y: 0, scale: 1, rotation: 0, opacity: 0 });
  available.push(el);
}

  });
}


    function surprise() {
      // evita più lanci sovrapposti
      if (tickInterval) {
        clearInterval(tickInterval);
        tickInterval = null;
      }

      showPopup.value = true;
      gsap.fromTo(".popup", { scale: 0, opacity: 0 }, { scale: 1, opacity: 1, duration: 0.45, ease: "back.out(1.7)" });
      setTimeout(() => {
        gsap.to(".popup", { scale: 0, opacity: 0, duration: 0.45, ease: "back.in(1.7)" });
        setTimeout(() => (showPopup.value = false), 450);
      }, 1800);

      // se pool non esiste (es. prima creazione), ricrea
      if (!pool || pool.length === 0) createPool();

      const start = Date.now();
      const screenW = window.innerWidth;
      const screenH = window.innerHeight;

      // emissione continua per 10s
      tickInterval = setInterval(() => {
        if (Date.now() - start > DURATION_MS) {
          clearInterval(tickInterval);
          tickInterval = null;
          return;
        }
        // un cuore ogni tick (puoi aumentare a 2-3 se vuoi più densità)
        launchHeart(screenW, screenH);
      }, TICK_MS);
    }

    return { title, message, message2, explosion, surprise, showPopup, audio };
  },
};
</script>

<style scoped>
h1, .popup {
  font-family: 'Pacifico', cursive;
}

p, button {
  font-family: 'Dancing Script', cursive;
}

.love-container {
  position: relative;
  top: 50%;
  transform: translateY(-50%);
  text-align: center;
  color: #ff0066;
  padding: 0 20px;
  z-index: 2;
}
h1 {
  font-size: 2.5rem;
  margin-bottom: 20px;
  text-shadow: 2px 2px 10px #ffb6c1;
  animation: heartbeat 2s infinite;
}
p {
  font-size: 1.5rem;
  margin-bottom: 30px;
  animation: heartbeat 2.5s infinite;
}
button {
  padding: 10px 20px;
  font-size: 1rem;
  background: #ff69b4;
  border: none;
  border-radius: 25px;
  color: white;
  cursor: pointer;
  transition: transform 0.2s;
}
button:hover {
  transform: scale(1.1);
}

/* container centrato e fissato alla finestra */
.explosion-container {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 0;
  height: 0;
  pointer-events: none;
  z-index: 50;
}

/* gli elementi sono assoluti e animati via transform (GPU) */
.heart-explosion {
  position: absolute;
  top: 0;
  left: 0;
  width: 28px;
  height: 28px;
  pointer-events: none;
  z-index: 60;
  transform: translate3d(0,0,0);
  will-change: transform, opacity;
  user-select: none;
}

/* popup e stile */
.popup {
  position: fixed;
  top: 20%;
  left: 50%;
  transform: translateX(-50%);
  background: #ff1493;
  color: white;
  padding: 15px 30px;
  border-radius: 20px;
  font-size: 3rem;
  font-weight: bold;
  text-align: center;
  z-index: 100;
  box-shadow: 0 0 20px rgba(255, 20, 147, 0.6);
  width: 60%;
  max-width: 600px;
}

@keyframes heartbeat {
  0%,
  100% {
    transform: scale(1);
  }
  20% {
    transform: scale(1.1);
  }
  40% {
    transform: scale(0.95);
  }
  60% {
    transform: scale(1.05);
  }
  80% {
    transform: scale(0.98);
  }
}
</style>
