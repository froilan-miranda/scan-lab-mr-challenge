<script setup>
import { ref } from 'vue';
import { onMounted } from 'vue';
import * as THREE from 'three';

const webGl = ref();

onMounted(() => {
  // scene setup
  console.log("component has mounted")
  const scene = new THREE.Scene();
  scene.backgroundColor = 0xffffff;
  scene.fog = new THREE.Fog(0xffffff, 0.0025, 50);

  // setup camera and basic renderer
  const camera = new THREE.PerspectiveCamera(75, window.innerWidth / (window.innerHeight*0.66), 0.1, 1000);
  camera.position.x = -3;
  camera.position.z = 8;
  camera.position.y = 2;

});
</script>

<template>
  <header>
  </header>

  <main>
    <canvas ref="webGl" class="webGl" />
  </main>
</template>

<style scoped>
</style>
