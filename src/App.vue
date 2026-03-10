<script setup lang="ts">
import ARViewer from './components/ARViewer.vue'
</script>

<template>
  <div class="app">
    <ARViewer />
  </div>
</template>

<style scoped>
.card {
  width: 90%;
  max-width: 400px;
  margin: 20px auto;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
}

.card-header {
  padding: 15px;
  text-align: center;
  
}
</style>