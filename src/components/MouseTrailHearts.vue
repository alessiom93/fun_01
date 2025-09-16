<template>
  <div class="animation-container">
    <div v-for="heart in hearts" :key="heart.id" class="cursor-heart"
         :style="{ left: heart.x+'px', top: heart.y+'px', opacity: heart.opacity, transform:'scale('+heart.scale+')' }">
      <svg viewBox="0 0 32 29.6" xmlns="http://www.w3.org/2000/svg">
        <path d="M23.6,0c-3.4,0-6.4,2.1-7.6,5.1C14.8,2.1,11.8,0,8.4,0C3.8,0,0,3.8,0,8.4
                 c0,9.3,16,21.2,16,21.2s16-11.9,16-21.2C32,3.8,28.2,0,23.6,0z" :fill="heart.color"/>
      </svg>
    </div>
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted } from "vue";

export default {
  name: "MouseTrailHearts",
  setup() {
    const hearts = ref([]);
    const colors = ["#ff1493","#ff4d6d","#ff69b4","#ff85c1","#c71585","#e75480"];
    const trailLimit = 15;
    let lastTime = 0;
    const interval = 40;

    const handleMouseMove = (e)=>{
      const now = Date.now();
      if(now-lastTime<interval) return;
      lastTime=now;

      const id = now+Math.random();
      const x = e.pageX-16;
      const y = e.pageY-16;
      const scale = 0.6 + Math.random()*0.5;

      hearts.value.push({id,x,y,scale,opacity:1,color:colors[Math.floor(Math.random()*colors.length)]});

      setTimeout(()=>{ const h=hearts.value.find(h=>h.id===id); if(h) h.opacity=0; },500);
      setTimeout(()=>{ hearts.value=hearts.value.filter(h=>h.id!==id); },800);

      if(hearts.value.length>trailLimit) hearts.value=hearts.value.slice(hearts.value.length-trailLimit);
    }

    onMounted(()=>{ window.addEventListener("mousemove",handleMouseMove); });
    onUnmounted(()=>{ window.removeEventListener("mousemove",handleMouseMove); });

    return { hearts };
  }
};
</script>

<style scoped>
.animation-container{
  position: fixed; top:0; left:0; width:100%; height:100%; pointer-events:none; z-index:1;
}
.cursor-heart{
  position:absolute; width:32px; height:30px; animation: floatTrail 0.8s ease-out forwards;
}
.cursor-heart svg{
  width:100%; height:100%; filter: drop-shadow(0 0 6px rgba(255,105,180,0.6));
}
@keyframes floatTrail{
  0%{transform:translateY(0) scale(1); opacity:1;}
  100%{transform:translateY(-20px) scale(1.2); opacity:0;}
}
</style>
