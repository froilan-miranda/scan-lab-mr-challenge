<script setup>
const props = defineProps(['output'])
</script>

<template>
  <div class="output-view">
    <h1>Output</h1>
    <h2>Intersection Vector3</h2>
    <ul>
      <li>x: {{ props.output.x }}</li>
      <li>y: {{ props.output.y }}</li>
      <li>z: {{ props.output.z }}</li>
    </ul>
  </div>
</template>

<style scoped>
li {
  list-style: none;
}
</style>
