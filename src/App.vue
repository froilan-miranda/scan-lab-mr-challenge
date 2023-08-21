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
  camera.position.x = 0;
  camera.position.z = 8;
  camera.position.y = 1.5;

  // setup the renderer and attach to canvas
  const canvas = webGl.value;
  const renderer = new THREE.WebGLRenderer({ canvas, antialias: true });
  renderer.outputColorSpace = THREE.SRGBColorSpace;
  renderer.shadowMap.enabled = true;
  renderer.shadowMap.type = THREE.VSMShadowMap;
  renderer.setSize(window.innerWidth, window.innerHeight*0.66);
  renderer.setClearColor(0xffffff);

  // add lights
  scene.add(new THREE.AmbientLight(0xffffff))
  const dirLight = new THREE.DirectionalLight(0xffffff)
  dirLight.position.set(5, 12, 8)
  dirLight.castShadow = true
  dirLight.intensity = 1;
  dirLight.shadow.camera.near = 0.1;
  dirLight.shadow.camera.far = 200;
  dirLight.shadow.camera.right = 10;
  dirLight.shadow.camera.left = -10;
  dirLight.shadow.camera.top = 10;
  dirLight.shadow.camera.bottom = -10;
  dirLight.shadow.mapSize.width = 512;
  dirLight.shadow.mapSize.height = 512;
  dirLight.shadow.radius = 4;
  dirLight.shadow.bias = -0.0005;
  scene.add(dirLight);

  // create a cube and torus knot and add them to the scene
  const cubeGeometry = new THREE.BoxGeometry();
  const cubeMaterial = new THREE.MeshPhongMaterial({ color: 0x0000FF });
  const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);

  cube.position.x = 1;
  cube.position.y = 0;
  cube.position.z = 4;
  //cube.castShadow = true;
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
