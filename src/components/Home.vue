<script setup lang="ts">
defineEmits(["openAR"]);
</script>

<template>
  <div class="home-container">
    <header class="hero">
      <div class="logo-circle">🌍</div>
      <h1>MundialScan <span class="accent">AR</span></h1>
      <p>Descubre datos mundialistas escaneando banderas reales</p>
    </header>

    <section class="instructions">
      <div class="step">
        <div class="step-icon">1</div>
        <div class="step-content">
          <p class="step-title">Busca una bandera</p>
          <p class="step-desc">
            Apunta a una bandera de los países participantes.
          </p>
        </div>
      </div>
      <div class="step">
        <div class="step-icon">2</div>
        <div class="step-content">
          <p class="step-title">Escanea y aprende</p>
          <p class="step-desc">
            Mantén el celular estable para ver la información.
          </p>
        </div>
      </div>
    </section>

    <nav class="bottom-nav">
      <button class="nav-btn active">
        <span class="icon">🏠</span>
        <span>Inicio</span>
      </button>

      <div class="cam-trigger-wrapper">
        <button @click="$emit('openAR')" class="cam-btn">
          <span class="icon">📸</span>
        </button>
      </div>

      <button class="nav-btn">
        <span class="icon">🏆</span>
        <span>Países</span>
      </button>
    </nav>
  </div>
</template>

<style scoped>
.home-container {
  padding: 40px 24px;
  background: #f8fafc;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.hero {
  text-align: center;
  margin-bottom: 40px;
}

.logo-circle {
  font-size: 50px;
  margin-bottom: 15px;
}

h1 {
  font-size: 2.2rem;
  font-weight: 800;
  color: #0f172a;
  margin: 0;
}

.accent {
  color: #10b981;
}

.instructions {
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.step {
  background: white;
  padding: 20px;
  border-radius: 20px;
  display: flex;
  gap: 16px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}

.step-icon {
  background: #10b981;
  color: white;
  width: 35px;
  height: 35px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  flex-shrink: 0;
}

.step-title {
  font-weight: 700;
  color: #1e293b;
  margin: 0;
}
.step-desc {
  font-size: 14px;
  color: #64748b;
  margin: 4px 0 0 0;
}

.bottom-nav {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 85px;
  background: white;
  display: flex;
  justify-content: space-around;
  align-items: center;
  box-shadow: 0 -5px 20px rgba(0, 0, 0, 0.05);
  padding-bottom: env(safe-area-inset-bottom);
}

.nav-btn {
  background: none;
  border: none;
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #94a3b8;
  font-size: 12px;
}

.nav-btn.active {
  color: #10b981;
}

.cam-trigger-wrapper {
  position: relative;
  top: -25px;
}

.cam-btn {
  width: 70px;
  height: 70px;
  background: #10b981;
  border-radius: 50%;
  border: 6px solid #f8fafc;
  font-size: 28px;
  color: white;
  box-shadow: 0 10px 15px -3px rgba(16, 185, 129, 0.4);
}
</style>
