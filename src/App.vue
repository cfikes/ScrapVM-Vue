<!-- App.vue -->
<template>
  <div id="app">
    <GlobalHeader />
    <div id="desktop">
      <WindowContainer
        v-for="(window, index) in windows"
        :key="index"
        v-bind="window"
        :zIndex="window.zIndex"
        @close="closeWindow(index)"
        @bringToFront="bringToFront(index)">
        
        <!-- Render AllApps component if window type is 'all-apps' -->
        <AllApps v-if="window.type === 'all-apps'" @launchApp="launchApp" />
      </WindowContainer>
    </div>
    <Toolbar :runningApps="runningApps" @openAppApps="openAppApps" />
  </div>
</template>

<script>
import GlobalHeader from './components/GlobalHeader.vue';
import WindowContainer from './components/WindowContainer.vue';
import Toolbar from './components/Toolbar.vue';
import AllApps from './components/AllApps.vue';

export default {
  components: {
    GlobalHeader,
    WindowContainer,
    Toolbar,
    AllApps,
  },
  data() {
    return {
      windows: [],
      runningApps: [],
      currentZIndex: 1,
    };
  },
  methods: {
    launchApp(app) {
      // Set an initial zIndex for each new window
      this.windows.push({
        x: 100,
        y: 100,
        width: 400,
        height: 300,
        title: app.title,
        appUrl: app.url,
        zIndex: this.getNextZIndex(), // Assign zIndex here
      });
    },
    openAppApps() {
      // Define 'All Apps' settings and launch
      const allAppsApp = {
        title: 'All Apps',
        type: 'all-apps', // Set type for All Apps specific window content
      };
      this.launchApp(allAppsApp);
    },
    closeWindow(index) {
      this.windows.splice(index, 1);
    },
    bringToFront(index) {
      // Update the zIndex of the window when brought to front
      this.windows[index].zIndex = this.getNextZIndex();
    },
    getNextZIndex() {
      return ++this.currentZIndex;
    },
  },
};
</script>
