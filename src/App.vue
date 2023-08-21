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

  // setup the renderer and attach to canvas
  const canvas = webGl.value;
  const renderer = new THREE.WebGLRenderer({ canvas, antialias: true });
  renderer.outputColorSpace = THREE.SRGBColorSpace;
  renderer.shadowMap.enabled = true;
  renderer.shadowMap.type = THREE.VSMShadowMap;
  renderer.setSize(window.innerWidth, window.innerHeight*0.66);
  renderer.setClearColor(0xffffff);

  // create a cube and torus knot and add them to the scene
  const cubeGeometry = new THREE.BoxGeometry();
  const cubeMaterial = new THREE.MeshBasicMaterial({
        color: 'green',
        transparent: true,
        opacity: 0.4
    });
  const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);

  cube.position.x = -1;
  cube.castShadow = true;
  scene.add(cube);

  renderer.render(scene, camera)

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
