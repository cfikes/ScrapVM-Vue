<template>
  <div id="toolbar">
    <i class="base-app-shortcut" id="allApps" @click="openAppApps()">
        <img src="/vfs/system/icons/application-w.png" class="app-icon"/>
    </i>
    <div v-for="app in runningApps" :key="app.title">
      <span>{{ app.title }}</span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    runningApps: Array,
  },
  methods: {
    openAppApps() {
      this.$emit('openAppApps');
    },
  },
};
</script>
