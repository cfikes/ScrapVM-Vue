<template>
    <div class="all-apps-container">
      <div class="app-grid">
        <div
          v-for="app in apps"
          :key="app.name"
          class="app-grid-item"
          @click="openApp(app)"
        >
          <div class="app-icon" :style="{ backgroundImage: `url(${app.icon})` }"></div>
          <span class="app-name">{{ app.name }}</span>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        apps: [
          {
            name: 'Calendar',
            icon: 'path/to/calendar-icon.png',
            url: '/apps/calendar.html',
            width: 500,
            height: 600,
          },
          {
            name: 'VoIP',
            icon: 'path/to/voip-icon.png',
            url: '/apps/voip.html',
            width: 400,
            height: 600,
          },
          // Add more apps as needed
        ],
      };
    },
    methods: {
      openApp(app) {
        this.$emit('launchApp', app);
      },
    },
  };
  </script>
  
  <style scoped>
  .all-apps-container {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
    height: 100%;
    background-color: var(--bg-color);
  }
  
  .app-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
    gap: 20px;
  }
  
  .app-grid-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: transform 0.2s ease;
  }
  
  .app-grid-item:hover {
    transform: scale(1.05);
  }
  
  .app-icon {
    width: 72px;
    height: 72px;
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center;
    border-radius: 8px;
    margin-bottom: 8px;
  }
  
  .app-name {
    font-size: 14px;
    text-align: center;
    color: var(--text-color);
  }
  </style>
  