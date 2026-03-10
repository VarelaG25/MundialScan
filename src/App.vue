<script lang="ts" setup>
import { ref, watch } from 'vue'
import ARViewer from './components/ARViewer.vue'

const isARActive = ref(false)

const toggleAR = () => {
  isARActive.value = !isARActive.value
}

// Limpieza radical al destruir
watch(isARActive, (active) => {
  if (!active) {
    // 1. Matar cámara
    const videos = document.querySelectorAll('video');
    videos.forEach(v => {
      if (v.srcObject) {
        (v.srcObject as MediaStream).getTracks().forEach(t => t.stop());
      }
      v.remove();
    });

    // 2. Eliminar basura del DOM (AR.js inyecta cosas fuera de la app)
    const garbage = ['#arjs-video', '.a-canvas', '.arjs-loader', 'a-scene', '.a-enter-vr'];
    garbage.forEach(s => document.querySelector(s)?.remove());

    // 3. Reset de estilos globales de A-Frame
    // document.documentElement.classList.remove('a-fullscreen');
    document.body.classList.remove('a-fullscreen');
    document.body.style.overflow = 'auto';
  }
})
</script>

<template>
  <div class="app-container">
    <div v-if="!isARActive" class="lobby">
      <div class="lobby-card">
        <h1>Flag Scanner AR</h1>
        <p>Escanea marcadores para ver banderas.</p>
        <button @click="toggleAR" class="btn-start">Iniciar Experiencia</button>
      </div>
    </div>

    <div v-else class="ar-wrapper">
      <ARViewer />
      <button @click="toggleAR" class="btn-exit">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="3">
          <line x1="18" y1="6" x2="6" y2="18"></line>
          <line x1="6" y1="6" x2="18" y2="18"></line>
        </svg>
      </button>
    </div>
  </div>
</template>

<style>
html, body { margin: 0; padding: 0; width: 100%; height: 100%; }

.lobby {
  display: flex; justify-content: center; align-items: center;
  height: 100vh; 
}

.lobby-card {
  padding: 30px;  border-radius: 20px;
  text-align: center; box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}

.btn-start {
  background: #10b981; color: white; border: none;
  padding: 12px 24px; border-radius: 10px; font-weight: bold; cursor: pointer;
}


.btn-exit {
  position: fixed; bottom: 30px; left: 50%; transform: translateX(-50%);
  z-index: 10000; background: #ef4444; border: none;
  width: 50px; height: 50px; border-radius: 50%; cursor: pointer;
}
</style>