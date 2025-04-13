<template>
  <div class="flowers-container">
    <div class="background-effect"></div>
    <div class="flower" v-for="n in 3" :key="n" :style="{ '--delay': `${n * 0.2}s` }">
      <div class="petals">
        <div class="petal" v-for="i in 6" :key="i" :style="{ '--rotation': `${i * 60}deg` }"></div>
      </div>
      <div class="center">
        <div class="stamen" v-for="i in 6" :key="i" :style="{ '--rotation': `${i * 60}deg` }"></div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.flowers-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 1000;
  display: flex;
  justify-content: space-around;
  align-items: flex-start;
  padding-top: 20px;
}

.background-effect {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: radial-gradient(circle at center, rgba(255, 255, 255, 0.1) 0%, rgba(255, 255, 255, 0) 70%);
  z-index: -1;
}

.flower {
  position: relative;
  width: 150px;
  height: 150px;
  opacity: 0;
  animation: bloom 1s ease-out forwards, fall 2s ease-in forwards;
  animation-delay: var(--delay), calc(var(--delay) + 1s);
  filter: drop-shadow(0 0 10px rgba(0, 0, 0, 0.1));
}

.petals {
  position: absolute;
  width: 100%;
  height: 100%;
}

.petal {
  position: absolute;
  width: 50px;
  height: 80px;
  background: linear-gradient(135deg, #ffffff, #f8f8f8);
  border-radius: 50% 50% 0 0;
  top: 35px;
  left: 50px;
  transform-origin: 50% 100%;
  transform: rotate(var(--rotation));
  animation: petalBloom 1s ease-out forwards;
  animation-delay: var(--delay);
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.15);
  border: 1px solid rgba(0, 0, 0, 0.05);
}

.petal::before {
  content: '';
  position: absolute;
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #ffffff, #f8f8f8);
  border-radius: 50% 50% 0 0;
  transform: scale(0.8);
  top: 0;
  left: 0;
}

.center {
  position: absolute;
  width: 30px;
  height: 30px;
  background: #ffd700;
  border-radius: 50%;
  top: 60px;
  left: 60px;
  animation: centerBloom 1s ease-out forwards;
  animation-delay: var(--delay);
  z-index: 2;
  box-shadow: 0 0 10px rgba(255, 215, 0, 0.3);
}

.stamen {
  position: absolute;
  width: 2px;
  height: 20px;
  background: #ffd700;
  top: -20px;
  left: 14px;
  transform-origin: 50% 100%;
  transform: rotate(var(--rotation));
  animation: stamenBloom 1s ease-out forwards;
  animation-delay: var(--delay);
  box-shadow: 0 0 5px rgba(255, 215, 0, 0.3);
}

.stamen::before {
  content: '';
  position: absolute;
  width: 8px;
  height: 8px;
  background: #ffd700;
  border-radius: 50%;
  top: -8px;
  left: -3px;
  box-shadow: 0 0 5px rgba(255, 215, 0, 0.3);
}

@keyframes bloom {
  0% {
    opacity: 0;
    transform: scale(0.5);
  }
  100% {
    opacity: 1;
    transform: scale(1);
  }
}

@keyframes fall {
  0% {
    transform: translateY(0);
  }
  100% {
    transform: translateY(calc(100vh - 200px));
  }
}

@keyframes petalBloom {
  0% {
    transform: rotate(var(--rotation)) scale(0);
  }
  100% {
    transform: rotate(var(--rotation)) scale(1);
  }
}

@keyframes centerBloom {
  0% {
    transform: scale(0);
  }
  100% {
    transform: scale(1);
  }
}

@keyframes stamenBloom {
  0% {
    transform: rotate(var(--rotation)) scale(0);
  }
  100% {
    transform: rotate(var(--rotation)) scale(1);
  }
}
</style> 