<template>
  <div class="ar-viewer-container">
    <div class="ui-layer">
      <div class="glass-card">
        <div class="status-dot" :class="{ 'detected': currentFlag !== 'Buscando...' }"></div>
        <div class="info">
          <span class="label">{{ currentFlag === 'Buscando...' ? 'Escaneando' : 'Detectado' }}</span>
          <h2 class="title">{{ currentFlag }}</h2>
        </div>
      </div>
      <button @click="$emit('close')" class="btn-close-ar">✕</button>
    </div>

    <a-scene embedded vr-mode-ui="enabled: false" renderer="logarithmicDepthBuffer: true; alpha: true;"
      arjs="sourceType: webcam; debugUIEnabled: false; uiScanning: no; trackingMethod: best;">
      <a-assets>
        <img v-for="c in countries" :key="'img-' + c.id" :id="c.id" :src="c.flagUrl">
      </a-assets>

      <a-marker v-for="c in countries" :key="'marker-' + c.id" type="pattern" :url="c.patternUrl"
        @markerFound="currentFlag = c.name" @markerLost="currentFlag = 'Buscando...'">
        <a-plane :src="'#' + c.id" position="0 0.1 0" rotation="-130 0 0" width="1.2" height="0.8">
        </a-plane>
      </a-marker>

      <a-entity camera></a-entity>
    </a-scene>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue';

const currentFlag = ref('Buscando...');
const countries = [
  { id: 'mexico', name: 'México', patternUrl: '/markers/pattern-mexico.patt', flagUrl: '/flags/mexico-flag.png' },
  { id: 'usa', name: 'USA', patternUrl: '/markers/pattern-usa.patt', flagUrl: '/flags/usa-flag.jpg' },
  { id: 'argentina', name: 'Argentina', patternUrl: '/markers/pattern-argentina.patt', flagUrl: '/flags/argentina-flag.jpg' }
];
</script>

<style scoped>
.ar-viewer-container {
  position: fixed;
  inset: 0;
  width: 100vw !important;
  height: 100vh !important;
  z-index: 9999;
  background: transparent !important;
  /* Evita el fondo blanco */
}

:deep(video#arjs-video) {
  width: 100vw !important;
  height: 100vh !important;
  object-fit: cover !important;
  position: fixed !important;
  top: 0 !important;
  left: 0 !important;
  z-index: -2 !important;
}

:deep(.a-canvas) {
  z-index: -1 !important;
}

.btn-close-ar {
  position: absolute;
  top: 20px;
  right: 20px;
  background: rgba(0,0,0,0.5);
  border: none;
  color: white;
  font-size: 24px;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  cursor: pointer;
}
</style>