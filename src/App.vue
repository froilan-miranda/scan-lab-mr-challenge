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
  //const camera = new THREE.PerspectiveCamera(75, window.innerWidth / (window.innerHeight*0.66), 0.1, 1000);
  const left = -3.2, right = 3.2,
  top2 = 2.4, bottom = -2.4,
  near = 0.01, far = 100;
  
  const camera = new THREE.OrthographicCamera(left, right, top2, bottom, near, far);
  camera.position.set(1, 1, 2);
  camera.lookAt(0, 0, 0);

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

  // create a cube add to the scene
  const cubeGeometry = new THREE.BoxGeometry();
  const cubeMaterial = new THREE.MeshPhongMaterial({ color: 0x000000 });
  const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);

  cube.position.x = 0;
  cube.position.y = 0;
  cube.position.z = 0;
  scene.add(cube);

  // create a line and add to scene
  const lineMaterial = new THREE.LineBasicMaterial({
    color: 0x00ff00
  });
  const points = [];
  points.push(new THREE.Vector3(-0.5, 0, 0));
  points.push(new THREE.Vector3(-1, -1, 0));
  const lineGeometry = new THREE.BufferGeometry().setFromPoints(points);
  const line = new THREE.Line(lineGeometry, lineMaterial);
  scene.add(line);

  //create sphere and add to scene
  const sphereGeometry = new THREE.SphereGeometry(0.1, 10, 10);
  const sphereMaterial = new THREE.MeshBasicMaterial({ color: 0x0000ff });
  const sphere = new THREE.Mesh(sphereGeometry, sphereMaterial);
  sphere.position.x = -0.5;
  sphere.position.y = 0;
  sphere.position.z = 0;
  scene.add(sphere);

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
