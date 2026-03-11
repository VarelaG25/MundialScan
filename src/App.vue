<script lang="ts" setup>
import { ref, watch, nextTick } from "vue";
import Home from "./components/Home.vue";
import ARViewer from "./components/ARViewer.vue";

const isARActive = ref(false);

watch(isARActive, async (active) => {
  if (!active) {
    await nextTick();
    // Limpieza agresiva de MindAR/A-Frame
    const sceneEl = document.querySelector("a-scene") as any;
    if (sceneEl?.systems["mindar-image-system"]) {
      sceneEl.systems["mindar-image-system"].stop();
    }

    document
      .querySelectorAll("video, canvas, a-scene, .mindar-ui-overlay")
      .forEach((el) => el.remove());
    document.body.style.backgroundColor = "#f1f5f9";
  } else {
    document.body.style.backgroundColor = "transparent";
  }
});
</script>

<template>
  <div class="app-root">
    <Home v-if="!isARActive" @openAR="isARActive = true" />
    <ARViewer v-else @close="isARActive = false" />
  </div>
</template>

<style>
/* Reset global mínimo */
body {
  margin: 0;
  font-family: "Inter", sans-serif;
  overflow-x: hidden;
}
</style>
