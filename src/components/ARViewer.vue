<template>
  <div class="ar-wrapper">
    <div class="card shadow-lg">
      <div class="card-header">
        <div class="icon-badge">
          <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M23 19a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h4l2-3h6l2 3h4a2 2 0 0 1 2 2z"/><circle cx="12" cy="13" r="4"/></svg>
        </div>
        <div class="header-text">
          <h3>Escáner de Bandera</h3>
          <span>Apunta al marcador impreso</span>
        </div>
      </div>

      <div class="ar-container" ref="arContainer">
        <div class="scan-overlay"></div> 
        
        <a-scene
          embedded
          arjs="sourceType: webcam; debugUIEnabled: false; detectionMode: mono_and_matrix; matrixCodeType: 3x3;"
          renderer="colorManagement: true;"
          vr-mode-ui="enabled: false"
        >
          <a-assets>
            <img id="banderaImg" src="/flags/mexico-flag.png">
          </a-assets>

          <a-marker type="pattern" url="/markers/pattern-mexico.patt">
            <a-plane 
              src="#banderaImg" 
              position="0 0.1 0" 
              rotation="-90 0 0"
              height="0.6" 
              width="1"
              animation="property: scale; from: 0 0 0; to: 1 1 1; dur: 800; easing: easeOutBack"
            ></a-plane>
          </a-marker>

          <a-entity camera></a-entity>
        </a-scene>
      </div>
      
      <div class="card-footer">
        <div class="status-indicator">
          <span class="pulse"></span>
          Listo para escanear
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ARCard",
  mounted() {
    const interval = setInterval(() => {
      const video = document.getElementById('arjs-video') || document.querySelector('video');
      const container = this.$refs.arContainer;
      const scene = this.$el.querySelector('a-scene');

      if (video && container) {
        container.appendChild(video);
        
        const styles = {
          "width": "100%",
          "height": "100%",
          "position": "absolute",
          "top": "70%",
          "left": "0",
          "margin": "0",
          "object-fit": "cover",
          "z-index": "1"
        };

        Object.keys(styles).forEach(prop => {
          video.style.setProperty(prop, styles[prop], "important");
        });

        if (scene) {
          scene.style.setProperty("width", "100%", "important");
          scene.style.setProperty("height", "100%", "important");
          scene.style.setProperty("position", "absolute", "important");
          // Elevamos z-index del scene para que el 3D no quede detrás del video
          scene.style.setProperty("z-index", "2", "important");
        }

        clearInterval(interval);
      }
    }, 100);
  }
};
</script>

<style>
/* --- CORE FIXES --- */
html.a-fullscreen, html.a-fullscreen body {
  overflow-x: hidden !important;
  overflow-y: auto !important;
  height: auto !important;
  position: static !important;
  background-color: #f4f7f6 !important; /* Color de fondo de tu app */
}

/* --- WRAPPER PARA CENTRADO --- */
.ar-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
  min-height: 100vh;
  font-family: 'Inter', -apple-system, sans-serif;
}

/* --- TARJETA MODERNA --- */
.card {
  width: 100%;
  max-width: 420px;
  background: #ffffff;
  border-radius: 24px;
  overflow: hidden;
  box-shadow: 0 20px 40px rgba(0,0,0,0.1);
  border: 1px solid rgba(255,255,255,0.3);
  display: flex;
  flex-direction: column;
  transition: transform 0.3s ease;
}

/* --- HEADER --- */
.card-header {
  padding: 20px;
  display: flex;
  align-items: center;
  gap: 15px;
  background: #fff;
  z-index: 10;
}

.icon-badge {
  background: #f0fdf4;
  color: #22c55e;
  padding: 10px;
  border-radius: 12px;
  display: flex;
}

.header-text h3 {
  margin: 0;
  font-size: 1.1rem;
  color: #1e293b;
  font-weight: 700;
}

.header-text span {
  font-size: 0.85rem;
  color: #64748b;
}

/* --- CONTENEDOR AR --- */
.ar-container {
  width: 100%;
  height: 350px; /* Un poco más alto para móvil */
  position: relative !important;
  overflow: hidden !important;
  background: #0f172a;
}

/* Línea de escaneo animada */
.scan-overlay {
  position: absolute;
  top: 0; width: 100%; height: 2px;
  background: linear-gradient(to right, transparent, #22c55e, transparent);
  z-index: 5;
  animation: scan 3s infinite linear;
}

@keyframes scan {
  0% { top: 0; }
  100% { top: 100%; }
}

/* --- FOOTER --- */
.card-footer {
  padding: 18px;
  background: #fff;
  text-align: center;
  border-top: 1px solid #f1f5f9;
}

.status-indicator {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  font-size: 0.9rem;
  color: #475569;
  font-weight: 500;
}

.pulse {
  width: 8px;
  height: 8px;
  background: #22c55e;
  border-radius: 50%;
  box-shadow: 0 0 0 0 rgba(34, 197, 94, 0.7);
  animation: pulse 1.5s infinite;
}

@keyframes pulse {
  0% { transform: scale(0.95); box-shadow: 0 0 0 0 rgba(34, 197, 94, 0.7); }
  70% { transform: scale(1); box-shadow: 0 0 0 10px rgba(34, 197, 94, 0); }
  100% { transform: scale(0.95); box-shadow: 0 0 0 0 rgba(34, 197, 94, 0); }
}

/* --- RESPONSIVE MOBILE --- */
@media (max-width: 480px) {
  .ar-wrapper { padding: 10px; }
  .card { border-radius: 20px; }
  .ar-container { height: 450px; } /* Más espacio en móviles para ver mejor */
}

/* Limpieza de Canvas/Video */
.ar-container video, .ar-container canvas {
  width: 100% !important;
  height: 100% !important;
  position: absolute !important;
  object-fit: cover !important;
}

.a-enter-vr { display: none !important; }
</style>