<template>
    <div
      class="window-container"
      :class="{ resizable: isResizable }"
      :style="containerStyle"
      @mousedown="bringToFront"
    >
      <!-- Window Header with Controls -->
      <div class="window-header" @mousedown.prevent="startDragging">
        <div class="window-header-left">
          <i class="fas fa-bars menu-window-button" @click="toggleSnapMenu"></i>
        </div>
        <div class="window-header-center">{{ title }}</div>
        <div class="window-header-right">
          <i class="fas fa-window-minimize minimize-button" @click="minimizeWindow"></i>
          <i
            :class="isMaximized ? 'fas fa-window-restore' : 'fas fa-window-maximize'"
            @click="toggleMaximizeWindow"
          ></i>
          <i class="fas fa-times close-button" @click="closeWindow"></i>
        </div>
      </div>
  
      <!-- Content Area -->
      <iframe v-if="!isMinimized" :src="appUrl" class="window-content"></iframe>
  
      <!-- Snap Menu Popup -->
      <div
        v-if="isSnapMenuOpen"
        id="popupMenu"
        class="popup"
        @click.stop
        style="position: absolute; top: 30px; left: 10px; z-index: 9000;"
      >
        <div v-for="(group, index) in snapGroups" :key="index" class="snap-group">
          <div
            v-for="snapType in group"
            :key="snapType"
            class="snap-option"
            :data-snap="snapType"
            @click="applySnap(snapType)"
          ></div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      x: Number,
      y: Number,
      width: Number,
      height: Number,
      title: String,
      appUrl: String,
      zIndex: Number,
    },
    data() {
      return {
        globalHeaderHeight: 40,
        snapGroups: [
          ['left-half', 'right-half'],
          ['left-two-thirds', 'right-one-third'],
          ['left-third', 'center-third', 'right-third'],
        ],
        localX: this.x,
        localY: this.y,
        localWidth: this.width,
        localHeight: this.height,
        originalX: this.x,
        originalY: this.y,
        originalWidth: this.width,
        originalHeight: this.height,
        isMinimized: false,
        isMaximized: false,
        isSnapMenuOpen: false,
        isDragging: false,
        offsetX: 0,
        offsetY: 0,
        isResizable: true,
      };
    },
    watch: {
      x(newX) {
        this.localX = newX;
      },
      y(newY) {
        this.localY = newY;
      },
      width(newWidth) {
        this.localWidth = newWidth;
      },
      height(newHeight) {
        this.localHeight = newHeight;
      },
    },
    computed: {
      containerStyle() {
        return {
          position: 'absolute',
          left: `${this.localX}px`,
          top: `${this.localY}px`,
          width: `${this.localWidth}px`,
          height: `${this.localHeight}px`,
          zIndex: this.zIndex,
        };
      },
    },
    methods: {
      toggleSnapMenu() {
        this.isSnapMenuOpen = !this.isSnapMenuOpen;
      },
      minimizeWindow() {
        this.isMinimized = !this.isMinimized;
      },
      toggleMaximizeWindow() {
        if (!this.isMaximized) {
          // Store the current position and size before maximizing
          this.originalX = this.localX;
          this.originalY = this.localY;
          this.originalWidth = this.localWidth;
          this.originalHeight = this.localHeight;
  
          // Maximize the window
          this.localX = 0;
          this.localY = this.globalHeaderHeight;
          this.localWidth = window.innerWidth;
          this.localHeight = window.innerHeight - this.globalHeaderHeight;
        } else {
          // Restore to the original position and size
          this.localX = this.originalX;
          this.localY = this.originalY;
          this.localWidth = this.originalWidth;
          this.localHeight = this.originalHeight;
        }
        this.isMaximized = !this.isMaximized;
      },
      closeWindow() {
        this.$emit('close');
      },
      bringToFront() {
        this.$emit('bringToFront');
      },
      applySnap(snapType) {
        this.isSnapMenuOpen = false;
        const headerHeight = this.globalHeaderHeight;
  
        switch (snapType) {
          case 'left-half':
            this.localX = 0;
            this.localY = headerHeight;
            this.localWidth = window.innerWidth / 2;
            this.localHeight = window.innerHeight - headerHeight;
            break;
          case 'right-half':
            this.localX = window.innerWidth / 2;
            this.localY = headerHeight;
            this.localWidth = window.innerWidth / 2;
            this.localHeight = window.innerHeight - headerHeight;
            break;
          case 'left-two-thirds':
            this.localX = 0;
            this.localY = headerHeight;
            this.localWidth = (window.innerWidth * 2) / 3;
            this.localHeight = window.innerHeight - headerHeight;
            break;
          case 'right-one-third':
            this.localX = (window.innerWidth * 2) / 3;
            this.localY = headerHeight;
            this.localWidth = window.innerWidth / 3;
            this.localHeight = window.innerHeight - headerHeight;
            break;
          case 'left-third':
            this.localX = 0;
            this.localY = headerHeight;
            this.localWidth = window.innerWidth / 3;
            this.localHeight = window.innerHeight - headerHeight;
            break;
          case 'center-third':
            this.localX = window.innerWidth / 3;
            this.localY = headerHeight;
            this.localWidth = window.innerWidth / 3;
            this.localHeight = window.innerHeight - headerHeight;
            break;
          case 'right-third':
            this.localX = (window.innerWidth * 2) / 3;
            this.localY = headerHeight;
            this.localWidth = window.innerWidth / 3;
            this.localHeight = window.innerHeight - headerHeight;
            break;
        }
      },
      startDragging(event) {
        this.offsetX = event.clientX - this.localX;
        this.offsetY = event.clientY - this.localY;
        this.isDragging = true;
  
        document.addEventListener('mousemove', this.handleDragging);
        document.addEventListener('mouseup', this.stopDragging);
      },
      handleDragging(event) {
        if (this.isDragging) {
          this.localX = event.clientX - this.offsetX;
          this.localY = event.clientY - this.offsetY;
        }
      },
      stopDragging() {
        this.isDragging = false;
        document.removeEventListener('mousemove', this.handleDragging);
        document.removeEventListener('mouseup', this.stopDragging);
      },
    },
  };
  </script>
  
  <style scoped>
  .window-container {
    position: absolute;
    border: 2px solid var(--border-color);
    background-color: var(--text-color);
    overflow: hidden;
    transform-origin: center center;
    border-radius: 8px;
    z-index: 10;
  }
  
  .window-container.resizable {
    resize: both;
  }
  
  .window-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: var(--primary-bg-color);
    color: var(--text-color);
    padding: 8px 10px;
    cursor: move;
  }
  
  .window-header-left,
  .window-header-center,
  .window-header-right {
    display: flex;
    align-items: center;
  }
  
  .window-header-center {
    flex-grow: 1;
    justify-content: center;
  }
  
  .window-header-right i {
    font-size: 16px;
    color: var(--text-color);
    margin-left: 8px;
    cursor: pointer;
  }
  
  .window-header-right i:hover {
    color: var(--highlight-color);
  }
  
  .window-content {
    width: 100%;
    height: calc(100% - 40px); /* Account for header height */
    border: none;
  }
  
  .popup {
    background-color: var(--popup-bg-color);
    padding: 10px;
    border: 1px solid var(--border-color);
    border-radius: 5px;
    box-shadow: 0 4px 8px var(--shadow-color);
  }
  
  .snap-group {
    display: flex;
    gap: 2px;
    margin-bottom: 5px;
  }
  
  .snap-option {
    width: 44px;
    height: 64px;
    background-color: var(--secondary-bg-color);
    border: 1px solid #888;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.2s;
  }
  
  .snap-option:hover {
    background-color: var(--highlight-color);
  }

    /* Split Styling */
    [data-snap="left-half"],
    [data-snap="right-half"] {
        width: 44px;
    }

    [data-snap="left-two-thirds"] {
        width: 50px;
    }

    [data-snap="right-one-third"] {
        width: 30px;
    }

    [data-snap="left-third"],
    [data-snap="center-third"],
    [data-snap="right-third"] {
        width: 29px;
    }

  </style>
  