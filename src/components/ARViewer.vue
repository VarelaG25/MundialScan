<script setup lang="ts">
import { ref, onMounted } from "vue";

const isDetected = ref(false);
const activeCountry = ref<any>(null);

// Datos de prueba que coinciden con el targetIndex 0 del archivo .mind de ejemplo
const countries = [
  { 
    id: 0, 
    name: "Target de Prueba Detectado", 
    desc: "Si ves esto y el modelo 3D, MindAR está configurado correctamente en tu proyecto." 
  }
];

onMounted(() => {
  const sceneEl = document.querySelector("a-scene");
  
  // MindAR dispara estos eventos en la escena
  sceneEl?.addEventListener("targetFound", (event: any) => {
    console.log("Target encontrado:", event.detail.targetIndex);
    activeCountry.value = countries[event.detail.targetIndex] || { name: "Desconocido", desc: "Sin descripción" };
    isDetected.value = true;
  });

  sceneEl?.addEventListener("targetLost", () => {
    console.log("Target perdido");
    isDetected.value = false;
  });
});
</script>

<template>
  <div class="ar-viewport">
    <button @click="$emit('close')" class="back-btn">✕</button>

    <div v-if="!isDetected" class="scan-overlay">
      <div class="focus-box"></div>
      <p>Apunta a la tarjeta de prueba...</p>
      <a href="https://cdn.jsdelivr.net/gh/hiukim/mind-ar-js@1.2.5/examples/image-tracking/assets/card-example/card.png" target="_blank" style="color: #10b981; pointer-events: auto; margin-top: 10px;">Ver imagen para escanear</a>
    </div>

    <Transition name="slide">
      <div v-if="isDetected" class="info-panel">
        <div class="drag-handle"></div>
        <h2>{{ activeCountry?.name }}</h2>
        <p>{{ activeCountry?.desc }}</p>
        <div class="status-badge">AR Activo</div>
      </div>
    </Transition>

    <a-scene
      mindar-image="imageTargetSrc: https://cdn.jsdelivr.net/gh/hiukim/mind-ar-js@1.2.5/examples/image-tracking/assets/card-example/card.mind; autoStart: true; uiScanning: no;"
      embedded
      color-space="sRGB"
      renderer="colorManagement: true, physicallyCorrectLights"
      vr-mode-ui="enabled: false"
      device-orientation-permission-ui="enabled: false"
    >
      <a-assets>
        <img id="cardImg" src="https://cdn.jsdelivr.net/gh/hiukim/mind-ar-js@1.2.5/examples/image-tracking/assets/card-example/card.png" />
        <a-asset-item id="avatarModel" src="https://cdn.jsdelivr.net/gh/hiukim/mind-ar-js@1.2.5/examples/image-tracking/assets/card-example/softmind/scene.gltf"></a-asset-item>
      </a-assets>

      <a-camera position="0 0 0" look-controls="enabled: false"></a-camera>

      <a-entity mindar-image-target="targetIndex: 0">
        <a-plane src="#cardImg" position="0 0 0" height="0.552" width="1" rotation="0 0 0"></a-plane>
        
        <a-gltf-model 
          src="#avatarModel" 
          rotation="0 0 0" 
          position="0 0 0.1" 
          scale="0.005 0.005 0.005"
          animation="property: position; to: 0 0.1 0.1; dur: 1000; easing: easeInOutQuad; loop: true; dir: alternate"
        ></a-gltf-model>
      </a-entity>
    </a-scene>
  </div>
</template>

<style scoped>
/* Tu CSS existente se mantiene igual */
.ar-viewport { position: fixed; inset: 0; background: #000; z-index: 1000; }
.back-btn { 
  position: absolute; top: 40px; left: 20px; z-index: 1100; 
  width: 45px; height: 45px; border-radius: 50%; border: none; 
  background: rgba(0, 0, 0, 0.5); color: white; font-size: 20px; cursor: pointer;
}
.scan-overlay {
  position: absolute; inset: 0; z-index: 1050;
  display: flex; flex-direction: column; align-items: center; justify-content: center;
  color: white; pointer-events: none; text-align: center;
}
.focus-box {
  width: 250px; height: 180px; border: 2px solid #10b981;
  border-radius: 20px; margin-bottom: 20px;
  box-shadow: 0 0 0 1000px rgba(0, 0, 0, 0.6);
}
.info-panel {
  position: absolute; bottom: 0; width: 100%; background: white;
  border-radius: 30px 30px 0 0; padding: 30px; z-index: 1100;
  box-shadow: 0 -10px 30px rgba(0, 0, 0, 0.3);
  color: #1e293b;
}
.drag-handle { width: 40px; height: 4px; background: #e2e8f0; border-radius: 2px; margin: -10px auto 20px auto; }
.status-badge { display: inline-block; background: #10b981; color: white; padding: 4px 12px; border-radius: 20px; font-size: 12px; margin-top: 10px; }

.slide-enter-active, .slide-leave-active { transition: transform 0.4s cubic-bezier(0.165, 0.84, 0.44, 1); }
.slide-enter-from, .slide-leave-to { transform: translateY(100%); }
</style>