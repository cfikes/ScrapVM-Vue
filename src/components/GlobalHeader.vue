<template>
  <header class="global-header">
    <div class="global-header-left">
        <i class="fas fa-computer systemMenuItem" id="systemMenuButton"></i>
        <div id="extraSystemMenus"></div>
    </div>
    <div class="global-header-center">ScrapWM</div>
    <div class="global-header-right">
      <i class="fas fa-phone"></i>
      <i class="fas fa-envelope"></i>
      <!-- Add other icons as needed -->
      <span class="system-clock">{{ clock }}</span>
    </div>
  </header>
</template>

<script>
export default {
  data() {
    return {
      clock: '',
    };
  },
  mounted() {
    this.updateClock();
    setInterval(this.updateClock, 1000);
  },
  methods: {
    updateClock() {
      const now = new Date();
      const hours = now.getHours();
      const minutes = now.getMinutes();
      const ampm = hours >= 12 ? 'PM' : 'AM';
      const displayHours = hours % 12 || 12;
      const displayMinutes = minutes < 10 ? '0' + minutes : minutes;
      this.clock = `${displayHours}:${displayMinutes} ${ampm}`;
    },
  },
};
</script>
