<script setup>
import { loadFull } from "tsparticles";
import particlesConfig from '@/assets/particles.json';

const particlesInit = async (engine) => {
  await loadFull(engine);
};

const particlesLoaded = async (container) => {
  console.log("Particles container loaded", container);
};
</script>

<template>
  <div>
    <Particles
      id="tsparticles"
      :particlesInit="particlesInit"
      :particlesLoaded="particlesLoaded"
      :options=particlesConfig
    />
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
 