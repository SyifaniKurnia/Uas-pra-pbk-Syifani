<template>
  <q-page class="flex flex-center">
    <q-card class="text-center q-pa-md card-container" style="max-width: 400px; position: relative; background-color: transparent;">
      <div ref="animatedText" class="text-h4">Bebas Tugas</div>
      <div class="custom-shape-divider-bottom">
        <svg data-name="Layer 1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1200 120" preserveAspectRatio="none">
          <path d="M321.39,56.44c58-10.79,114.16-30.13,172-41.86,82.39-16.72,168.19-17.73,250.45-.39C823.78,31,906.67,72,985.66,92.83c70.05,18.48,146.53,26.09,214.34,3V0H0V27.35A600.21,600.21,0,0,0,321.39,56.44Z" class="shape-fill"></path>
        </svg>
      </div>
    </q-card>
  </q-page>
</template>

<script>
import anime from 'animejs/lib/anime.es.js';

export default {
  name: 'Task5',
  mounted() {
    anime({
      targets: this.$refs.animatedText,
      keyframes: [
        { translateX: -10 },
        { translateX: 10 },
        { translateX: 0 },
      ],
      duration: 1000,
      easing: 'easeInOutSine',
      loop: true,
    });
  },
};
</script>

<style scoped>
.q-page {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}

.card-container {
  position: relative;
  background-color: transparent !important; /* Important to override Quasar default */
}

.text-h4 {
  font-size: 2rem;
  font-weight: bold;
  position: relative;
  z-index: 1;
  color: aliceblue;
}

.custom-shape-divider-bottom {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  overflow: hidden;
  line-height: 0;
  z-index: 0;
}

.custom-shape-divider-bottom svg {
  position: relative;
  display: block;
  width: calc(100% + 1.3px);
  height: 73px;
}

.custom-shape-divider-bottom .shape-fill {
  fill: #5bc3e8;
}
</style>
