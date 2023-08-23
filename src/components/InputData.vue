<script setup>
  defineProps(["modelValue"])
</script>

<template>
  <div class="input-controls">
    <h1>Form Inputs</h1>
    <div class="cube-controls">
      <h2>Cube (Center Vector3)</h2>
      <label for="cube-x">
        x: <input :value.number="modelValue.cube.x" type="number" @input="modelValue.cube.x = parseInt($event.target.value)" />
      </label>
      <label for="cube-y">
        y: <input :value="modelValue.cube.y" type="number" @input="modelValue.cube.y = parseInt($event.target.value)" />
      </label>
      <label for="cube-z">
        z: <input :value="modelValue.cube.z" type="number" @input="modelValue.cube.z = parseInt($event.target.value)" />
      </label>
    </div>
    <div class="line-controls">
      <h2>Line Segments</h2>
      <div class="line-start-controls">
        <h3>Start Vector3</h3>
        <label for="line-start-x">
          x: <input :value="modelValue.line.start.x" type="number" @input="modelValue.line.start.x = parseInt($event.target.value)" />
        </label>
        <label for="line-start-y">
          y: <input :value="modelValue.line.start.y" type="number" @input="modelValue.line.start.y = parseInt($event.target.value)" />
        </label>
        <label for="line-start-z">
          z: <input :value="modelValue.line.start.z" type="number" @input="modelValue.line.start.z = parseInt($event.target.value)" />
        </label>
      </div>
      <div class="line-end-controls">
        <h3>End Vector3</h3>
        <label for="line-end-x">
          x: <input :value="modelValue.line.end.x" type="number" @input="modelValue.line.end.x = parseInt($event.target.value)" />
        </label>
        <label for="line-end-y">
          y: <input :value="modelValue.line.end.y" type="number" @input="modelValue.line.end.y = parseInt($event.target.value)" />
        </label>
        <label for="line-end-z">
          z: <input :value="modelValue.line.end.z" type="number" @input="modelValue.line.end.z = parseInt($event.target.value)" />
        </label>
      </div>
    </div>
  </div>
</template>
<style scoped>
label {
  display: block;
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
</style>
