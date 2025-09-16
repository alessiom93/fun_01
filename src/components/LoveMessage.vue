<template>
  <div class="love-container">
    <h1 ref="title">Per te, amore mio ❤️</h1>
    <p ref="message">Ogni giorno con te è speciale… Ti amo tantissimo!</p>
    <button @click="surprise">Clicca per un messaggio speciale 💌</button>

    <div ref="explosion" class="explosion-container"></div>
    <div v-if="showPopup" class="popup">Ti amo ❤️❤️❤️</div>
    <audio ref="audio" src="/music/il_mio_canto_libero.mp3" autoplay loop></audio>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
import { gsap } from "gsap";

export default {
  setup(){
    const title=ref(null);
    const message=ref(null);
    const explosion=ref(null);
    const audio=ref(null);
    const showPopup=ref(false);

    onMounted(()=>{
      gsap.from(title.value,{y:-50,opacity:0,duration:1,ease:"bounce.out"});
      gsap.from(message.value,{y:50,opacity:0,duration:1.5,delay:0.5});
      audio.value.play().catch(()=>{});
    });

    const surprise=()=>{
      showPopup.value=true;
      gsap.fromTo(".popup",{scale:0,opacity:0},{scale:1,opacity:1,duration:0.5,ease:"back.out(1.7)"});
      setTimeout(()=>{
        gsap.to(".popup",{scale:0,opacity:0,duration:0.5,ease:"back.in(1.7)"});
        setTimeout(()=>showPopup.value=false,500);
      },2000);

      const colors=["#ff1493","#ff4d6d","#ff69b4","#ff85c1","#c71585","#e75480"];
      for(let i=0;i<25;i++){
        const heart=document.createElement("div");
        heart.className="heart-explosion";
        heart.innerHTML=`<svg viewBox="0 0 32 29.6" width="25" height="25">
          <path d="M23.6,0c-3.4,0-6.4,2.1-7.6,5.1C14.8,2.1,11.8,0,8.4,0C3.8,0,0,3.8,0,8.4
          c0,9.3,16,21.2,16,21.2s16-11.9,16-21.2C32,3.8,28.2,0,23.6,0z" fill="${colors[Math.floor(Math.random()*colors.length)]}"/>
        </svg>`;
        explosion.value.appendChild(heart);

        const x=Math.random()*200-100;
        const y=Math.random()*-200-50;
        const scale=Math.random()*0.7+0.5;

        gsap.to(heart,{x,y,scale,opacity:0,duration:1+Math.random(),ease:"power1.out",onComplete:()=>heart.remove()});
      }
    };

    return { title,message,explosion,surprise,showPopup,audio };
  }
};
</script>

<style scoped>
.love-container{
  position: relative; top:50%; transform:translateY(-50%);
  text-align:center; color:#ff0066; padding:0 20px; z-index:2;
}
h1{ font-size:2.5rem; margin-bottom:20px; text-shadow:2px 2px 10px #ffb6c1; animation:heartbeat 2s infinite; }
p{ font-size:1.5rem; margin-bottom:30px; animation:heartbeat 2.5s infinite; }
button{ padding:10px 20px; font-size:1rem; background:#ff69b4; border:none; border-radius:25px; color:white; cursor:pointer; transition:transform 0.2s; }
button:hover{ transform:scale(1.1); }

.explosion-container{ position:absolute; top:50%; left:50%; pointer-events:none; z-index:5; }
.heart-explosion{ position:absolute; top:0; left:0; width:25px; height:25px; pointer-events:none; z-index:5; }
.popup{
  position:fixed; top:20%; left:50%; transform:translateX(-50%);
  background:#ff1493; color:white; padding:15px 30px; border-radius:20px; font-size:1.5rem;
  text-align:center; z-index:10; box-shadow:0 0 20px rgba(255,20,147,0.6);
}

@keyframes heartbeat{
  0%,100%{transform:scale(1);}
  20%{transform:scale(1.1);}
  40%{transform:scale(0.95);}
  60%{transform:scale(1.05);}
  80%{transform:scale(0.98);}
}
</style>
