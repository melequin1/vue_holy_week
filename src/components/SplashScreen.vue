<script setup>
import { ref, onMounted } from 'vue';

const showSplash = ref(true);

onMounted(() => {
  setTimeout(() => {
    showSplash.value = false;
  }, 3000); // Show splash screen for 3 seconds
});
</script>

<template>
  <transition name="fade">
    <div v-if="showSplash" class="splash-screen">
      <div class="splash-content">
        <h1>Welcome to</h1>
        <h2>The Sacred Triduum</h2>
        <p class="invitation">A journey through the holiest days of the Christian year</p>
        <div class="cross">
          <div class="vertical"></div>
          <div class="horizontal"></div>
        </div>
      </div>
    </div>
  </transition>
</template>

<style scoped>
.splash-screen {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  background: linear-gradient(135deg, #2c3e50, #34495e);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.splash-content {
  text-align: center;
  color: white;
  padding: 2rem;
}

h1 {
  font-size: 2.5rem;
  margin-bottom: 0.5rem;
  font-weight: 300;
  letter-spacing: 2px;
}

h2 {
  font-size: 3rem;
  margin-bottom: 1.5rem;
  font-weight: 400;
  letter-spacing: 3px;
}

.invitation {
  font-size: 1.2rem;
  margin-bottom: 2rem;
  font-style: italic;
  opacity: 0.9;
}

.cross {
  position: relative;
  width: 100px;
  height: 100px;
  margin: 0 auto;
}

.vertical {
  position: absolute;
  width: 4px;
  height: 100%;
  background-color: white;
  left: 50%;
  transform: translateX(-50%);
  animation: fadeIn 1.5s ease-out;
}

.horizontal {
  position: absolute;
  width: 100%;
  height: 4px;
  background-color: white;
  top: 50%;
  transform: translateY(-50%);
  animation: fadeIn 1.5s ease-out;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 1s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: scale(0.8);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}
</style> 