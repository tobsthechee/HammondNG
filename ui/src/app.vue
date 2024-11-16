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
        bodyBg: '#f9f7f5', // Default light theme background
        textColor: '#444',  // Default light theme text color
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
        this.currentTheme.bodyBg = '#1e1e1e'; // Dark theme background
        this.currentTheme.textColor = '#f9f9f9'; // Dark theme text color
      } else {
        this.currentTheme.bodyBg = '#f9f7f5'; // Light theme background
        this.currentTheme.textColor = '#444'; // Light theme text color
      }
      localStorage.setItem('theme', theme); // Save theme preference
    },
    toggleTheme() {
      const currentTheme = localStorage.getItem('theme') || 'light';
      const newTheme = currentTheme === 'light' ? 'dark' : 'light';
      this.setTheme(newTheme); // Toggle theme
    },
  },
}
</script>

<style scoped>
/* Add any additional styles here */
</style>
