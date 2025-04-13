<script setup>
import { holyWeekData } from './data/holyWeekData';
import { ref, computed } from 'vue';
import EasterFlowers from './components/EasterFlowers.vue';
import SplashScreen from './components/SplashScreen.vue';

const currentDayIndex = ref(0);
const isScriptureCollapsed = ref(false);
const isFamilyActivityCollapsed = ref(false);
const isSacredArtCollapsed = ref(false);

const days = Object.entries(holyWeekData).map(([key, value]) => ({
  key,
  ...value
}));

const currentDay = computed(() => days[currentDayIndex.value]);

const formatDayName = (key) => {
  const names = {
    palmSunday: 'Palm Sunday',
    holyMonday: 'Holy Monday',
    holyTuesday: 'Holy Tuesday',
    holyWednesday: 'Holy Wednesday',
    maundyThursday: 'Maundy Thursday',
    goodFriday: 'Good Friday',
    holySaturday: 'Holy Saturday',
    easterSunday: 'Easter Sunday'
  };
  return names[key] || key;
};

const nextDay = () => {
  if (currentDayIndex.value < days.length - 1) {
    currentDayIndex.value++;
  }
};

const previousDay = () => {
  if (currentDayIndex.value > 0) {
    currentDayIndex.value--;
  }
};

const isEasterSunday = computed(() => currentDay.value.key === 'easterSunday');

const toggleSection = (section) => {
  switch(section) {
    case 'scripture':
      isScriptureCollapsed.value = !isScriptureCollapsed.value;
      break;
    case 'familyActivity':
      isFamilyActivityCollapsed.value = !isFamilyActivityCollapsed.value;
      break;
    case 'sacredArt':
      isSacredArtCollapsed.value = !isSacredArtCollapsed.value;
      break;
  }
};

const printActivities = () => {
  // Create a new window for printing
  const printWindow = window.open('', '_blank');
  
  // Get the content we want to print
  const content = `
    <!DOCTYPE html>
    <html>
    <head>
      <title>Family Activities - ${formatDayName(currentDay.value.key)}</title>
      <style>
        body {
          font-family: Georgia, serif;
          line-height: 1.6;
          color: #333;
          padding: 20px;
        }
        .print-container {
          max-width: 1000px;
          margin: 0 auto;
        }
        h1 {
          text-align: center;
          color: #2c3e50;
          margin-bottom: 30px;
        }
        .activities-grid {
          display: grid;
          grid-template-columns: repeat(3, 1fr);
          gap: 20px;
        }
        .activity-card {
          border: 1px solid #ccc;
          padding: 15px;
          break-inside: avoid;
        }
        .age-group {
          color: #2c3e50;
          margin-bottom: 10px;
          font-size: 1.2em;
        }
        .activity-title {
          color: #34495e;
          margin-bottom: 10px;
          font-weight: bold;
        }
      </style>
    </head>
    <body>
      <div class="print-container">
        <h1>Family Activities for ${formatDayName(currentDay.value.key)}</h1>
        <div class="activities-grid">
          <div class="activity-card">
            <h2 class="age-group">Ages 0-2</h2>
            <div class="activity-title">${currentDay.value.familyActivity.toddlers.title}</div>
            <p>${currentDay.value.familyActivity.toddlers.description}</p>
          </div>
          <div class="activity-card">
            <h2 class="age-group">Ages 3-9</h2>
            <div class="activity-title">${currentDay.value.familyActivity.children.title}</div>
            <p>${currentDay.value.familyActivity.children.description}</p>
          </div>
          <div class="activity-card">
            <h2 class="age-group">Ages 10+</h2>
            <div class="activity-title">${currentDay.value.familyActivity.olderChildren.title}</div>
            <p>${currentDay.value.familyActivity.olderChildren.description}</p>
          </div>
        </div>
      </div>
    </body>
    </html>
  `;

  // Write the content to the new window
  printWindow.document.write(content);
  printWindow.document.close();

  // Wait for content to load then print
  printWindow.onload = function() {
    printWindow.print();
    printWindow.onafterprint = function() {
      printWindow.close();
    };
  };
};
</script>

<template>
  <div class="app">
    <SplashScreen />
    <EasterFlowers v-if="isEasterSunday" />
    <header class="header">
      <h1>Holy Week 2025</h1>
      <p class="subtitle">A Journey Through the Sacred Triduum</p>
    </header>

    <main class="main-content">
      <div class="navigation">
        <button 
          @click="previousDay" 
          :disabled="currentDayIndex === 0"
          class="nav-button"
        >
          Previous Day
        </button>
        <button 
          @click="nextDay" 
          :disabled="currentDayIndex === days.length - 1"
          class="nav-button"
        >
          Next Day
        </button>
      </div>

      <transition name="day-card" mode="out-in">
        <div class="day-card" :key="currentDay.key">
          <h2>{{ formatDayName(currentDay.key) }}</h2>
          <p class="date">{{ currentDay.date }}</p>
          
          <div class="section scripture">
            <div class="section-header" @click="toggleSection('scripture')">
              <h3>Scripture Reading</h3>
              <button class="toggle-button">
                {{ isScriptureCollapsed ? '▼' : '▲' }}
              </button>
            </div>
            <div class="section-content" v-if="!isScriptureCollapsed">
              <p class="reference">{{ currentDay.scripture.reference }}</p>
              <p class="text">{{ currentDay.scripture.text }}</p>
            </div>
          </div>

          <div class="section family-activity">
            <div class="section-header" @click="toggleSection('familyActivity')">
              <h3>Family Activities</h3>
              <div class="header-actions">
                <button class="print-button" @click.stop="printActivities">
                  <span class="print-icon">🖨️</span>
                  Print Activities
                </button>
                <button class="toggle-button">
                  {{ isFamilyActivityCollapsed ? '▼' : '▲' }}
                </button>
              </div>
            </div>
            <div class="section-content" v-if="!isFamilyActivityCollapsed">
              <div class="activity-columns">
                <div class="activity-column">
                  <h4>Ages 0-2</h4>
                  <div class="activity-card">
                    <h5>{{ currentDay.familyActivity.toddlers.title }}</h5>
                    <p>{{ currentDay.familyActivity.toddlers.description }}</p>
                  </div>
                </div>
                <div class="activity-column">
                  <h4>Ages 3-9</h4>
                  <div class="activity-card">
                    <h5>{{ currentDay.familyActivity.children.title }}</h5>
                    <p>{{ currentDay.familyActivity.children.description }}</p>
                  </div>
                </div>
                <div class="activity-column">
                  <h4>Ages 10+</h4>
                  <div class="activity-card">
                    <h5>{{ currentDay.familyActivity.olderChildren.title }}</h5>
                    <p>{{ currentDay.familyActivity.olderChildren.description }}</p>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Print-only version of family activities -->
          <div class="print-only-activities">
            <h2>Family Activities for {{ formatDayName(currentDay.key) }}</h2>
            <div class="print-activity-columns">
              <div class="print-activity-column">
                <h4>Ages 0-2</h4>
                <div class="print-activity-card">
                  <h5>{{ currentDay.familyActivity.toddlers.title }}</h5>
                  <p>{{ currentDay.familyActivity.toddlers.description }}</p>
                </div>
              </div>
              <div class="print-activity-column">
                <h4>Ages 3-9</h4>
                <div class="print-activity-card">
                  <h5>{{ currentDay.familyActivity.children.title }}</h5>
                  <p>{{ currentDay.familyActivity.children.description }}</p>
                </div>
              </div>
              <div class="print-activity-column">
                <h4>Ages 10+</h4>
                <div class="print-activity-card">
                  <h5>{{ currentDay.familyActivity.olderChildren.title }}</h5>
                  <p>{{ currentDay.familyActivity.olderChildren.description }}</p>
                </div>
              </div>
            </div>
          </div>

          <div class="section sacred-art">
            <div class="section-header" @click="toggleSection('sacredArt')">
              <h3>Sacred Art</h3>
              <button class="toggle-button">
                {{ isSacredArtCollapsed ? '▼' : '▲' }}
              </button>
            </div>
            <div class="section-content" v-if="!isSacredArtCollapsed">
              <h4>{{ currentDay.sacredArt.title }}</h4>
              <p class="artist">{{ currentDay.sacredArt.artist }} ({{ currentDay.sacredArt.year }})</p>
              <p class="description">{{ currentDay.sacredArt.description }}</p>
              <div class="art-image">
                <img :src="currentDay.sacredArt.imageUrl" :alt="currentDay.sacredArt.title">
              </div>
            </div>
          </div>
        </div>
      </transition>
    </main>

    <footer class="footer">
      <p>© 2025 Holy Week Reflections</p>
    </footer>
  </div>
</template>

<style>
:root {
  --primary-color: #2c3e50;
  --secondary-color: #34495e;
  --accent-color: #7f8c8d;
  --text-color: #333;
  --background-color: #f5f5f5;
  --card-background: #ffffff;
  --border-color: #e0e0e0;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Georgia', serif;
  line-height: 1.6;
  color: var(--text-color);
  background-color: var(--background-color);
}

.app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.header {
  background-color: var(--primary-color);
  color: white;
  text-align: center;
  padding: 2rem 1rem;
  margin-bottom: 2rem;
}

.header h1 {
  font-size: 2.5rem;
  margin-bottom: 0.5rem;
}

.subtitle {
  font-style: italic;
  opacity: 0.9;
}

.main-content {
  flex: 1;
  padding: 0 1rem;
  max-width: 1200px;
  margin: 0 auto;
  width: 100%;
}

.navigation {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 2rem;
}

.nav-button {
  padding: 0.5rem 1rem;
  background-color: var(--primary-color);
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.nav-button:hover:not(:disabled) {
  background-color: var(--secondary-color);
}

.nav-button:disabled {
  background-color: var(--accent-color);
  cursor: not-allowed;
  opacity: 0.7;
}

.day-card {
  max-width: 800px;
  margin: 0 auto;
  background-color: var(--card-background);
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
  transition: transform 0.3s ease;
  position: relative;
  overflow: hidden;
}

.day-card-enter-active,
.day-card-leave-active {
  transition: all 0.5s ease;
}

.day-card-enter-from {
  opacity: 0;
  transform: translateX(30px);
}

.day-card-leave-to {
  opacity: 0;
  transform: translateX(-30px);
}

.day-card-enter-to,
.day-card-leave-from {
  opacity: 1;
  transform: translateX(0);
}

.day-card:hover {
  transform: translateY(-5px);
}

.day-card h2 {
  color: var(--primary-color);
  margin-bottom: 0.5rem;
  font-size: 1.5rem;
}

.date {
  color: var(--accent-color);
  font-style: italic;
  margin-bottom: 1rem;
}

.section {
  margin-bottom: 1.5rem;
}

.section h3 {
  color: var(--secondary-color);
  margin-bottom: 0.5rem;
  font-size: 1.2rem;
}

.section h4 {
  color: var(--primary-color);
  margin: 0.5rem 0;
}

.reference {
  font-weight: bold;
  margin-bottom: 0.5rem;
}

.text {
  font-style: italic;
}

.artist {
  font-style: italic;
  color: var(--accent-color);
}

.description {
  margin: 0.5rem 0;
}

.art-image {
  margin-top: 1rem;
}

.art-image img {
  width: 100%;
  height: auto;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.footer {
  background-color: var(--primary-color);
  color: white;
  text-align: center;
  padding: 1rem;
  margin-top: auto;
}

.activity-columns {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.5rem;
  margin-top: 1rem;
}

.activity-column {
  display: flex;
  flex-direction: column;
}

.activity-column h4 {
  color: var(--primary-color);
  margin-bottom: 0.5rem;
  text-align: center;
  font-size: 1.1rem;
}

.activity-card {
  background-color: var(--card-background);
  border-radius: 8px;
  padding: 1rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  height: 100%;
}

.activity-card h5 {
  color: var(--secondary-color);
  margin-bottom: 0.5rem;
  font-size: 1rem;
}

.activity-card p {
  font-size: 0.9rem;
  line-height: 1.4;
}

@media (max-width: 768px) {
  .header h1 {
    font-size: 2rem;
  }
  
  .navigation {
    flex-direction: column;
  }

  .activity-columns {
    grid-template-columns: 1fr;
    gap: 1rem;
  }
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  padding: 0.5rem 0;
}

.toggle-button {
  background: none;
  border: none;
  font-size: 1.2rem;
  color: var(--primary-color);
  cursor: pointer;
  padding: 0.5rem;
}

.section-content {
  transition: all 0.3s ease;
}

.header-actions {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.print-button {
  background: none;
  border: none;
  color: var(--primary-color);
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.9rem;
  padding: 0.5rem;
  border-radius: 4px;
  transition: background-color 0.3s ease;
}

.print-button:hover {
  background-color: rgba(0, 0, 0, 0.05);
}

.print-icon {
  font-size: 1.2rem;
}

/* Print-only content (hidden by default) */
.print-only-activities {
  display: none;
}

/* Print styles */
@media print {
  /* Hide everything except print version */
  .app * {
    display: none;
  }

  .print-only-activities {
    display: block;
    padding: 20px;
    max-width: 100%;
  }

  .print-only-activities h2 {
    text-align: center;
    margin-bottom: 30px;
    color: black;
  }

  .print-activity-columns {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
  }

  .print-activity-column h4 {
    margin-bottom: 10px;
    color: black;
  }

  .print-activity-card {
    padding: 15px;
    border: 1px solid #ccc;
    break-inside: avoid;
  }

  .print-activity-card h5 {
    margin-bottom: 10px;
    color: black;
  }

  .print-activity-card p {
    color: black;
  }

  /* Basic print settings */
  @page {
    margin: 2cm;
    size: portrait;
  }

  body {
    background: white;
  }
}
</style>
