<script lang="ts" setup>
import { ref } from 'vue'

// Estado para controlar si la cámara está abierta o no
// (Asegúrate de que tu componente Cam exponga o maneje este estado)
const isCamOpen = ref(false)

const handleCamToggle = (state: boolean) => {
    isCamOpen.value = state
}
</script>


<template>
    <div class="app-wrapper">
        <main v-if="!isCamOpen" class="lobby-container">
            <div class="hero-section">
                <h1 class="title">MundialScan AR</h1>
                <p class="subtitle">Escanea los marcadores para ver las banderas en realidad aumentada.</p>
            </div>

            <div class="card-instructions">
                <span>Paso 1: Busca un marcador</span>
                <span>Paso 2: Presiona el botón central</span>
            </div>
        </main>

        <Cam @status-change="handleCamToggle" />

        <nav v-if="!isCamOpen" class="mobile-navbar">
            <button class="nav-item">
                <span class="icon">🏠</span>
                <span class="nav-label">Inicio</span>
            </button>

            <div class="nav-cam-wrapper">
                <div class="cam-button-bg">
                    <button class="cam-trigger">
                        <span class="icon-cam">📸</span>
                    </button>
                </div>
            </div>

            <button class="nav-item">
                <span class="icon">🏆</span>
                <span class="nav-label">Países</span>
            </button>
        </nav>
    </div>
</template>

<style>
/* Reset y Base */
.app-wrapper {
  background-color: #f8fafc;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.lobby-container {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 20px;
  text-align: center;
}

/* Navbar Inferior */
.mobile-navbar {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 70px;
  background: white;
  display: flex;
  justify-content: space-around;
  align-items: center;
  box-shadow: 0 -2px 15px rgba(0, 0, 0, 0.05);
  padding-bottom: env(safe-area-inset-bottom); /* Soporte para iPhone Notch */
  z-index: 50;
}

.nav-item {
  border: none;
  background: none;
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #64748b;
  font-size: 12px;
}

/* El "Círculo" de la cámara */
.nav-cam-wrapper {
  position: relative;
  top: -20px; /* Lo eleva sobre el navbar */
}

.cam-button-bg {
  background: #10b981;
  width: 65px;
  height: 65px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  border: 5px solid #f8fafc; /* Crea el efecto de corte */
  box-shadow: 0 4px 10px rgba(16, 185, 129, 0.4);
}

.cam-trigger {
  border: none;
  background: transparent;
  font-size: 28px;
  cursor: pointer;
}

.icon-cam {
  filter: drop-shadow(0 2px 2px rgba(0,0,0,0.1));
}

/* Tipografía */
.title { font-size: 2rem; font-weight: 800; color: #1e293b; margin-bottom: 8px; }
.subtitle { color: #64748b; max-width: 300px; }

.card-instructions {
  margin-top: 40px;
  background: white;
  padding: 20px;
  border-radius: 16px;
  display: flex;
  flex-direction: column;
  gap: 10px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}
</style>