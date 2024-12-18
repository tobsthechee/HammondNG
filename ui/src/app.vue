<template>
  <div id="app" :style="{ backgroundColor: currentTheme.bodyBg, color: currentTheme.textColor }">
    <button @click="toggleTheme">Toggle Theme</button>
    <RouterView :key="$route.fullPath" />
  </div>
</template>

<script>
import appConfig from '@src/app.config'
import store from '@state/store'

export default {
  page: {
    titleTemplate(title) {
      title = typeof title === 'function' ? title(this.$store) : title
      return title ? `${title} | ${appConfig.title}` : appConfig.title
    },
  },
  data() {
    return {
      currentTheme: {
        bodyBg: '#f9f7f5', // Light theme background
        textColor: '#ff4500',  // Light theme text color
        headerBg: '#f9f7f5', // Light theme header background
        headerTextColor: '#ff4500', // Light theme header text color
        linkColor: '#ff4500', // Light theme link color
        buttonBg: '#ff4500', // Light theme button background
        buttonTextColor: '#ffffff', // Light theme button text color
      },
    };
  },
  created() {
    window.addEventListener('resize', this.onResize);
    this.setTheme(localStorage.getItem('theme') || 'light'); // Set initial theme
  },
  destroyed() {
    window.removeEventListener('resize', this.onResize);
  },
  methods: {
    onResize(e) {
      store.dispatch('utils/checkSize').then((isMobile) => {});
    },
    setTheme(theme) {
      if (theme === 'dark') {
        this.currentTheme.bodyBg = '#1e1e1e';
        this.currentTheme.textColor = '#ff4500';
        this.currentTheme.headerBg = '#333';
        this.currentTheme.headerTextColor = '#ffffff';
        this.currentTheme.linkColor = '#39a275'; // Ensure link color is readable on dark
        this.currentTheme.buttonBg = '#39a275';
        this.currentTheme.buttonTextColor = '#ffffff';
      } else {
        this.currentTheme.bodyBg = '#f9f7f5';
        this.currentTheme.textColor = '#ff4500';
        this.currentTheme.headerBg = '#f9f7f5';
        this.currentTheme.headerTextColor = '#35495e';
        this.currentTheme.linkColor = '#39a275';
        this.currentTheme.buttonBg = '#39a275';
        this.currentTheme.buttonTextColor = '#ffffff';
      }
      localStorage.setItem('theme', theme);
    },
    toggleTheme() {
      const currentTheme = localStorage.getItem('theme') || 'light';
      const newTheme = currentTheme === 'light' ? 'dark' : 'light';
      this.setTheme(newTheme);
    },
  },
}
</script>

<style scoped>
/* Add any additional styles here for the buttons, links, headers, etc. */

button {
  background-color: var(--button-bg);
  color: var(--button-text-color);
}

a {
  color: var(--link-color);
}

header {
  background-color: var(--header-bg);
  color: var(--header-text-color);
}
</style>
