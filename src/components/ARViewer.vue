<script setup lang="ts">
import { ref, onMounted } from "vue";

const isDetected = ref(false);
const activeCountry = ref<any>(null);

// 1. CONFIGURA AQUÍ TUS PAÍSES
// El orden debe ser el mismo en el que subiste las imágenes al compilador de MindAR
const countries = [
  {
    id: 0,
    name: "México",
    desc: "Sede del Mundial 2026. ¡Tierra de fútbol!",
    model: "#modelo-mexico", // Referencia al ID en <a-assets>
  },
  {
    id: 1,
    name: "Argentina",
    desc: "Actual campeón del mundo. ¡La Scaloneta!",
    model: "#modelo-argentina",
  },
];

onMounted(() => {
  const sceneEl = document.querySelector("a-scene");

  sceneEl?.addEventListener("targetFound", (event: any) => {
    const index = event.detail.targetIndex;
    activeCountry.value = countries[index];
    isDetected.value = true;
  });

  sceneEl?.addEventListener("targetLost", () => {
    isDetected.value = false;
  });
});
</script>

<template>
  <div class="ar-viewport">
    <button @click="$emit('close')" class="back-btn">✕</button>

    <div v-if="!isDetected" class="scan-overlay">
      <div class="focus-box"></div>
      <p>Apunta a una bandera registrada</p>
    </div>

    <Transition name="slide">
      <div v-if="isDetected" class="info-panel">
        <div class="drag-handle"></div>
        <span class="status-badge">País Detectado</span>
        <h2>{{ activeCountry?.name }}</h2>
        <p>{{ activeCountry?.desc }}</p>
      </div>
    </Transition>

    <a-scene
      mindar-image="imageTargetSrc: targets/targets.mind; autoStart: true; uiScanning: no;"
      embedded
      color-space="sRGB"
      renderer="colorManagement: true, physicallyCorrectLights"
      vr-mode-ui="enabled: false"
      device-orientation-permission-ui="enabled: false"
    >
      <a-assets>
        <a-asset-item
          id="modelo-mexico"
          src="models/mexico.gltf"
        ></a-asset-item>
        <a-asset-item
          id="modelo-argentina"
          src="models/argentina.gltf"
        ></a-asset-item>
      </a-assets>

      <a-camera position="0 0 0" look-controls="enabled: false"></a-camera>

      <a-entity mindar-image-target="targetIndex: 0">
        <a-gltf-model
          src="#modelo-mexico"
          rotation="0 0 0"
          position="0 0 0.1"
          scale="0.05 0.05 0.05"
        ></a-gltf-model>
      </a-entity>

      <a-entity mindar-image-target="targetIndex: 1">
        <a-gltf-model
          src="#modelo-argentina"
          rotation="0 0 0"
          position="0 0 0.1"
          scale="0.05 0.05 0.05"
        ></a-gltf-model>
      </a-entity>
    </a-scene>
  </div>
</template>

<style scoped>
/* Tu CSS existente se mantiene igual */
.ar-viewport {
  position: fixed;
  inset: 0;
  background: #000;
  z-index: 1000;
}
.back-btn {
  position: absolute;
  top: 40px;
  left: 20px;
  z-index: 1100;
  width: 45px;
  height: 45px;
  border-radius: 50%;
  border: none;
  background: rgba(0, 0, 0, 0.5);
  color: white;
  font-size: 20px;
  cursor: pointer;
}
.scan-overlay {
  position: absolute;
  inset: 0;
  z-index: 1050;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: white;
  pointer-events: none;
  text-align: center;
}
.focus-box {
  width: 250px;
  height: 180px;
  border: 2px solid #10b981;
  border-radius: 20px;
  margin-bottom: 20px;
  box-shadow: 0 0 0 1000px rgba(0, 0, 0, 0.6);
}
.info-panel {
  position: absolute;
  bottom: 0;
  width: 100%;
  background: white;
  border-radius: 30px 30px 0 0;
  padding: 30px;
  z-index: 1100;
  box-shadow: 0 -10px 30px rgba(0, 0, 0, 0.3);
  color: #1e293b;
}
.drag-handle {
  width: 40px;
  height: 4px;
  background: #e2e8f0;
  border-radius: 2px;
  margin: -10px auto 20px auto;
}
.status-badge {
  display: inline-block;
  background: #10b981;
  color: white;
  padding: 4px 12px;
  border-radius: 20px;
  font-size: 12px;
  margin-top: 10px;
}

.slide-enter-active,
.slide-leave-active {
  transition: transform 0.4s cubic-bezier(0.165, 0.84, 0.44, 1);
}
.slide-enter-from,
.slide-leave-to {
  transform: translateY(100%);
}
</style>
