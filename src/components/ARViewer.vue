<template>
  <div class="ar-ui">
    <div class="glass-card">
      <div class="status-dot" :class="{ 'detected': currentFlag !== 'Buscando...' }"></div>
      <div class="info">
        <span class="label">{{ currentFlag === 'Buscando...' ? 'Escaneando' : 'Detectado' }}</span>
        <h2 class="title">{{ currentFlag }}</h2>
      </div>
    </div>
  </div>

  <a-scene 
    vr-mode-ui="enabled: false" 
    renderer="logarithmicDepthBuffer: true; alpha: true;"
    arjs="sourceType: webcam; debugUIEnabled: false; uiScanning: no;"
  >
    <a-assets>
      <img v-for="c in countries" :key="c.id" :id="c.id" :src="c.flagUrl">
    </a-assets>

    <a-marker v-for="c in countries" :key="c.id" type="pattern" :url="c.patternUrl"
      @markerFound="currentFlag = c.name" @markerLost="currentFlag = 'Buscando...'">
      <a-plane :src="'#' + c.id" position="0 0.1 0" rotation="-90 0 0" height="0.6" width="1"></a-plane>
    </a-marker>

    <a-entity camera></a-entity>
  </a-scene>
</template>

<script>
export default {
  data() {
    return {
      currentFlag: 'Buscando...',
      countries: [
        { id: 'mexico', name: 'México', patternUrl: '/markers/pattern-mexico.patt', flagUrl: '/flags/mexico-flag.png' },
        { id: 'usa', name: 'USA', patternUrl: '/markers/pattern-usa.patt', flagUrl: '/flags/usa-flag.jpg' },
        { id: 'argentina', name: 'Argentina', patternUrl: '/markers/pattern-argentina.patt', flagUrl: '/flags/argentina-flag.jpg' }
      ]
    };
  }
};
</script>

<style scoped>
.ar-ui {
  position: fixed; top: 0; left: 0; width: 100%;
  z-index: 100; pointer-events: none;
  display: flex; justify-content: center; padding-top: 20px;
}

.glass-card {
  pointer-events: auto; background: rgba(255, 255, 255, 0.9);
  padding: 10px 20px; border-radius: 15px; display: flex; align-items: center; gap: 10px;
}

.status-dot { width: 10px; height: 10px; border-radius: 50%; background: #94a3b8; }
.status-dot.detected { background: #10b981; }
.label { font-size: 10px; display: block; color: #666; }
.title { margin: 0; font-size: 16px; }

/* Forzado de cámara pantalla completa sin desplazamiento */

</style>