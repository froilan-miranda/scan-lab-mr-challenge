<script setup>
import { ref } from 'vue';
import { onMounted } from 'vue';
import * as THREE from 'three';

const webGl = ref();
const output = ref({})
const controlState = ref({
  cube: {
    x: 0,
    y: 0,
    z: 0
  },
  spheere: {
    x: 0,
    y: 0,
    z: 0
  },
  line: {
    start: {
      x: -5,
      y: 2.5,
      z: 0
    },   
    end: {
      x: -12.5,
      y: -4,
      z: 0
    }
  }
})

onMounted(() => {
  // scene setup
  console.log("component has mounted")
  const scene = new THREE.Scene();
  scene.backgroundColor = 0xffffff;

  const cWidth = window.innerWidth;
  const cHeight = window.innerHeight * 0.6;

  const aspect = cWidth / cHeight;
  const viewSize = 40;

  // setup camera and basic renderer
  const left = -aspect * viewSize / 2, 
    right = aspect * viewSize / 2,
    top = viewSize / 2, 
    bottom = -viewSize / 2,
    near = -200, far = 200;
  
  const camera = new THREE.OrthographicCamera(left, right, top, bottom, near, far);
  camera.position.set(-0.2, 0.2, 1)
  camera.lookAt(0, 0, 0);

  // setup the renderer and attach to canvas
  const canvas = webGl.value;
  const renderer = new THREE.WebGLRenderer({ canvas, antialias: true });
  renderer.outputColorSpace = THREE.SRGBColorSpace;
  renderer.shadowMap.enabled = true;
  renderer.shadowMap.type = THREE.VSMShadowMap;
  renderer.setSize(cWidth, cHeight);
  renderer.setClearColor(0xffffff);
  renderer.sortObjects = false;

  // add lights
  scene.add(new THREE.AmbientLight(0xffffff, 0.5))
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

  // create a line and add to scene
  const lineMaterial = new THREE.LineBasicMaterial({
    color: 0x00ff00
  });
  const points = [];
  const start = new THREE.Vector3(controlState.value.line.start.x, controlState.value.line.start.y, controlState.value.line.start.z);
  const end = new THREE.Vector3(controlState.value.line.end.x, controlState.value.line.end.y, controlState.value.line.end.z);
  points.push(start);
  points.push(end);
  const lineGeometry = new THREE.BufferGeometry().setFromPoints(points);
  const line = new THREE.Line(lineGeometry, lineMaterial);
  scene.add(line);

  // create a ray for collision testing
  const ray = new THREE.Ray();

  //create sphere and add to scene
  const sphereGeometry = new THREE.SphereGeometry(0.05, 10, 10);
  const sphereMaterial = new THREE.MeshPhongMaterial({ color: 0x0000ff });
  sphereMaterial.transparent = true;
  sphereMaterial.opacity = 0.7;
  const sphere = new THREE.Mesh(sphereGeometry, sphereMaterial);
  sphere.scale.set(10, 10, 10)
  sphere.visible = false;
  scene.add(sphere);

  // create a cube add to the scene
  const cubeGeometry = new THREE.BoxGeometry();
  const cubeMaterial = new THREE.MeshPhongMaterial({ color: 0x666666 });
  cubeMaterial.transparent = true;
  cubeMaterial.opacity = 0.7;
  const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);
  cube.scale.set(10, 10, 10)
  cube.geometry.computeBoundingBox();

  scene.add(cube);

  // create bounding box for cube
  const box = new THREE.Box3().setFromObject(cube);

  function updateCubePosition(obj, pos) {
    obj.position.x = pos.x;
    obj.position.y = pos.y;
    obj.position.z = pos.z;
  }

  function updateLinePosition(){
    points.length = 0;
    start.set(controlState.value.line.start.x, controlState.value.line.start.y, controlState.value.line.start.z);
    end.set(controlState.value.line.end.x, controlState.value.line.end.y, controlState.value.line.end.z);
    points.push(start);
    points.push(end);
    lineGeometry.setFromPoints(points);
    line.geometry.verticesNeedUpdate = true;
  }

  function animate() {
    requestAnimationFrame(animate);
    renderer.render(scene, camera)
    
    updateCubePosition(cube, controlState.value.cube);

    box.copy(cube.geometry.boundingBox).applyMatrix4(cube.matrixWorld);

    updateLinePosition();

    ray.set(start, end.clone().sub(start).normalize());

    const intersectPoint = new THREE.Vector3();
    
    if(ray.intersectBox(box, intersectPoint)){
      sphere.position.copy(intersectPoint);
      sphere.visible = true;
      output.value.x = intersectPoint.x;
      output.value.y = intersectPoint.y;
      output.value.z = intersectPoint.z;
    }else{
      sphere.visible = false;
      output.value.x = '';
      output.value.y = '';
      output.value.z = '';
    }
  }
  animate();
});
</script>

<template>
  <header>
  </header>

  <main>
    <div class="viewport">
      <canvas ref="webGl" class="webGl" />
    </div>
    <div class="input-output">
      <div class="input-controls">
        <h1>Form Inputs</h1>
        <div class="cube-controls">
          <h2>Cube (Center Vector3)</h2>
          <label for="cube-x">
            x: <input v-model.number="controlState.cube.x" type="number" />
          </label>
          <label for="cube-y">
            y: <input v-model.number="controlState.cube.y" type="number" />
          </label>
          <label>
            z: <input v-model.number="controlState.cube.z" type="number" />
          </label>
        </div>
        <div class="line-controls">
          <h2>Line Segments</h2>
          <div class="line-start-controls">
            <h3>Start Vector3</h3>
            <label for="line-start-x">
              x: <input v-model.number="controlState.line.start.x" type="number" />
            </label>
            <label for="line-start-y">
              y: <input v-model.number="controlState.line.start.y" type="number" />
            </label>
            <label for="line-start-z">
              z: <input v-model.number="controlState.line.start.z" type="number" />
            </label>
          </div>
          <div class="line-end-controls">
            <h3>End Vector3</h3>
            <label for="line-end-x">
              x: <input v-model.number="controlState.line.end.x" type="number" />
            </label>
            <label for="line-end-y">
              y: <input v-model.number="controlState.line.end.y" type="number" />
            </label>
            <label for="line-end-z">
              z: <input v-model.number="controlState.line.end.z" type="number" />
            </label>
          </div>
        </div>
      </div>
      <div class="output-view">
        <h1>Output</h1>
        <h2>Intersection Vector3</h2>
        <ul>
          <li>x: {{ output.x }}</li>
          <li>y: {{ output.y }}</li>
          <li>z: {{ output.z }}</li>
        </ul>
      </div>
    </div>
  </main>
</template>

<style scoped>
label {
  display: block;
}
li {
  list-style: none;
}
.viewport {
  border: 1px solid black;
}
.input-output {
  display: flex;
  padding: 1rem;
}
.input-controls {
  flex-grow: 1;
}
.line-controls {
  margin-top: 1rem;
}
.line-end-controls {
  margin-top: 0.5rem;
}
.output-view {
  flex-grow: 1;
}
</style>
