<template>
  <div class="main-container">
    <div class="min-h-screen flex mx-auto justify-center items-center text-center">
      <div class="text-3xl md:text-7xl font-bold">Turn on your imagination</div>
    </div>

    <section ref="trigger" class="h-screen w-screen overflow-hidden relative">
      <div ref="moveContainer" class="flex h-full w-fit">
        <div v-for="(img, index) in images" :key="index" class="panel w-screen h-screen flex-shrink-0">
          <img :src="img.src" class="object-cover w-full h-full" :alt="img.alt">
        </div>
      </div>
    </section>
  </div>
</template>

<script setup>
import { onMounted, onUnmounted, ref, nextTick } from 'vue';
import LocomotiveScroll from 'locomotive-scroll';
import gsap from 'gsap';
import ScrollTrigger from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

const images = [
  { src: '/img/nature_221-wallpaper-3840x2160.jpg', alt: 1 },
  { src: '/img/derevo_kamni_svet_1359768_1920x1200.jpg', alt: 2 },
  { src: '/img/ozero_derevia_gory_1345936_1920x1200.jpg', alt: 3 },
  { src: '/img/domik_les_leto_123013_1920x1200.jpg', alt: 4 },
  { src: '/img/les_gory_luna_121180_1920x1200.jpg', alt: 5 },
  { src: '/img/ozero_gory_derevia_129959_1920x1200.jpg', alt: 6 },
  { src: '/img/severnoe_siianie_avrora_zima_122503_1920x1200.jpg', alt: 7 },
  { src: '/img/plantatsiia_ris_selskoe_hoziajstvo_128917_1920x1200.jpg', alt: 8 },
];

const trigger = ref(null);
const moveContainer = ref(null);
let locomotiveInstance = null;
let ctx = null;

const leaves = [
  { left: '6%', top: '18%', speed: 0.3, scale: 0.6 },
  { left: '20%', top: '45%', speed: 0.45, scale: 0.75 },
  { left: '38%', top: '25%', speed: 0.35, scale: 0.65 },
  { left: '60%', top: '50%', speed: 0.4, scale: 0.8 },
  { left: '78%', top: '30%', speed: 0.28, scale: 0.55 },
  { left: '88%', top: '65%', speed: 0.5, scale: 0.7 },
];

onMounted(async () => {
  await nextTick();
  
  locomotiveInstance = new LocomotiveScroll({
    el: document.querySelector('.main-container'),
    smooth: true,
  });

  ctx = gsap.context(() => {
    const panels = gsap.utils.toArray('.panel');
    const xDist = moveContainer.value.scrollWidth - window.innerWidth;

    gsap.to(moveContainer.value, {
      x: -xDist,
      ease: "none",
      scrollTrigger: {
        trigger: trigger.value,
        pin: true,
        scrub: 1,
        start: "top top",
        end: () => "+=" + xDist,
        invalidateOnRefresh: true,
        onUpdate: () => {
          locomotiveInstance.scrollTo('scroll', { duration: 0, disableLerp: true });
        }
      }
    });
  }, trigger.value);

  ScrollTrigger.refresh();
});

onUnmounted(() => {
  if (ctx) ctx.revert();
  if (locomotiveInstance) locomotiveInstance.destroy();
});
</script>

<style>
@import 'locomotive-scroll/dist/locomotive-scroll.css';

.leaf .icon {
  width: 120px;
  height: auto;
  display: block;
}

.leaf {
  opacity: 0.75;
  pointer-events: none;
}
</style>