<template>
  <div class="hearts-fall">
    <div v-for="h in hearts" :key="h.id" class="fall-heart"
         :style="{ left: h.x+'px', animationDuration: h.duration+'s', animationDelay: h.delay+'s', transform:'scale('+h.scale+')' }">
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

    onMounted(()=>{
      for(let i=0;i<25;i++){
        hearts.value.push({
          id:i,
          x: Math.random()*window.innerWidth,
          duration: 6 + Math.random()*5,
          delay: Math.random()*5,
          scale: 0.5 + Math.random()*0.8,
          color: colors[Math.floor(Math.random()*colors.length)]
        });
      }
    });

    return { hearts };
  }
};
</script>

<style scoped>
.hearts-fall{
  position: fixed; top:0; left:0; width:100%; height:100%;
  pointer-events:none; z-index:1;
}
.fall-heart{
  position:absolute;
  top:-50px;
  width:32px; height:30px;
  animation: fall linear infinite;
}
.fall-heart svg{
  width:100%; height:100%;
  filter: drop-shadow(0 0 6px rgba(255,105,180,0.6));
}
@keyframes fall{
  0%{transform:translateY(0) scale(1); opacity:1;}
  100%{transform:translateY(110vh) scale(1.2); opacity:0;}
}
</style>
