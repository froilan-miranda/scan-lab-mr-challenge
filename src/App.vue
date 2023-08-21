<script setup>
import { ref } from 'vue'
import * as THREE from 'three'

const webGl = ref();

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
