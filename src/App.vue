<script lang="ts" setup>
import { ref, watch, nextTick } from 'vue'
import ARViewer from './components/ARViewer.vue'

const isARActive = ref(false)
const toggleAR = () => { isARActive.value = !isARActive.value }

watch(isARActive, async (active) => {
  if (!active) {
    await nextTick();

    // Limpieza de videos y elementos de AR.js
    const videos = document.querySelectorAll('video');
    videos.forEach(v => {
      if (v.srcObject) (v.srcObject as MediaStream).getTracks().forEach(t => t.stop());
      v.remove();
    });

    const garbage = ['#arjs-video', '.a-canvas', 'a-scene', '.arjs-loader'];
    garbage.forEach(s => document.querySelectorAll(s).forEach(el => el.remove()));

    // RESET DE POSICIÓN (Por si AR.js movió el body)
    const resetStyles = () => {
      document.body.style.setProperty('margin-left', '0', 'important');
      document.body.style.setProperty('background-color', '#f1f5f9', 'important');
      document.documentElement.style.setProperty('margin-left', '0', 'important');
      window.scrollTo(0, 0);
    };

    resetStyles();
    setTimeout(resetStyles, 100);
  } else {
    // Mientras AR está activo, el body DEBE ser transparente
    document.body.style.setProperty('background-color', 'transparent', 'important');
  }
})
</script>

<template>
  <div class="app-container">
    <template v-if="!isARActive">
      <main class="lobby">
        <div class="hero-card">
          <h1 class="title">MundialScan AR</h1>
          <p class="subtitle">Descubre las banderas en realidad aumentada.</p>
        </div>
        <div class="instruction-list">
          <div class="step"><span>1</span> Busca un marcador oficial</div>
          <div class="step"><span>2</span> Presiona el botón de cámara</div>
        </div>
      </main>

      <nav class="navbar">
        <button class="nav-btn"><span>🏠</span><small>Inicio</small></button>
        <div class="cam-trigger-wrapper">
          <button @click="toggleAR" class="cam-btn-main">
            <svg viewBox="0 0 24 24" width="30" height="30" stroke="white" stroke-width="2" fill="none">
              <path d="M23 19a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h4l2-3h6l2 3h4a2 2 0 0 1 2 2z" />
              <circle cx="12" cy="13" r="4" />
            </svg>
          </button>
        </div>
        <button class="nav-btn"><span>🏆</span><small>Países</small></button>
      </nav>
    </template>

    <ARViewer v-else @close="toggleAR" />
  </div>
</template>

<style scoped>
/* Estilos del lobby y navbar que ya tienes, pero asegurando el ancho */
.app-container {
  width: 100%;
  overflow: hidden;
}

.navbar {
  position: fixed;
  bottom: 0;
  left: 0;
  /* Aseguramos que se pegue a la izquierda */
  width: 100%;
  height: 80px;
  background: white;
  display: flex;
  justify-content: space-around;
  z-index: 100;
}

/* Estilos Globales Críticos */
html,
body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  background-color: #f1f5f9;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.app-container {
  width: 100%;
  min-height: 100vh;
}

/* Lobby */
.lobby {
  padding: 60px 24px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.hero-card {
  text-align: center;
  margin-bottom: 40px;
}

.title {
  font-size: 2.2rem;
  font-weight: 800;
  color: #1e293b;
  margin: 0;
}

.subtitle {
  color: #64748b;
  margin-top: 10px;
  font-size: 1.1rem;
}

.instruction-list {
  width: 100%;
  max-width: 400px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.step {
  background: white;
  padding: 16px;
  border-radius: 16px;
  display: flex;
  align-items: center;
  gap: 16px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
}

.step-num {
  background: #10b981;
  color: white;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
}

/* Navbar */
.navbar {
  position: fixed;
  bottom: 0;
  width: 100%;
  height: 85px;
  background: white;
  display: flex;
  justify-content: space-around;
  align-items: center;
  box-shadow: 0 -4px 20px rgba(0, 0, 0, 0.05);
  z-index: 100;
  padding-bottom: env(safe-area-inset-bottom);
}

.nav-btn {
  border: none;
  background: none;
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #94a3b8;
}

.nav-icon {
  font-size: 24px;
}

.nav-text {
  font-size: 12px;
  font-weight: 600;
  margin-top: 4px;
}

.cam-trigger-wrapper {
  position: relative;
  top: -28px;
}

.cam-btn-main {
  width: 72px;
  height: 72px;
  background: #10b981;
  border: 6px solid #f1f5f9;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 8px 20px rgba(16, 185, 129, 0.4);
  cursor: pointer;
}
</style>