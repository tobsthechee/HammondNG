<script>
import { authComputed } from '@state/helpers'
import { mapGetters } from 'vuex'
import NavBarRoutes from './nav-bar-routes.vue'

export default {
  components: { NavBarRoutes },
  data() {
    return {
      persistentNavRoutes: [
        {
          name: 'home',
          title: 'Home',
        },
      ],
      loggedInNavRoutes: [
        {
          name: 'quickEntries',
          title: () => 'Quick Entries',
          badge: () => this.unprocessedQuickEntries.length,
        },
        {
          name: 'import',
          title: () => 'Import',
        },
        {
          name: 'settings',
          title: 'Settings',
        },
        {
          name: 'logout',
          title: 'Log out',
        },
      ],
      loggedOutNavRoutes: [
        {
          name: 'login',
          title: 'Log in',
        },
      ],
      adminNavRoutes: [
        {
          name: 'site-settings',
          title: 'Site Settings',
        },
        {
          name: 'users',
          title: 'Users',
        },
      ],
      currentTheme: {
        bodyBg: '#f9f7f5', // Default light theme background
        textColor: '#ff4500', // Default light theme text color
        headerBg: '#f9f7f5', // Light theme header background
        headerTextColor: '#ff4500', // Light theme header text color
        linkColor: '#ff4500', // Light theme link color
        buttonBg: '#ff4500', // Light theme button background
        buttonTextColor: '#ffffff', // Light theme button text color
      },
    }
  },
  computed: {
    ...authComputed,
    ...mapGetters('vehicles', ['unprocessedQuickEntries']),
    isAdmin() {
      return this.loggedIn && this.currentUser.role === 'ADMIN'
    },
  },
  methods: {
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
      localStorage.setItem('theme', theme); // Save theme preference
      this.applyTheme();
    },
    toggleTheme() {
      const currentTheme = localStorage.getItem('theme') || 'light';
      const newTheme = currentTheme === 'light' ? 'dark' : 'light';
      this.setTheme(newTheme); // Toggle theme
    },
    applyTheme() {
      // Apply theme colors dynamically to the document's root for global styles
      const root = document.documentElement;
      root.style.setProperty('--body-bg', this.currentTheme.bodyBg);
      root.style.setProperty('--text-color', this.currentTheme.textColor);
      root.style.setProperty('--header-bg', this.currentTheme.headerBg);
      root.style.setProperty('--header-text-color', this.currentTheme.headerTextColor);
      root.style.setProperty('--link-color', this.currentTheme.linkColor);
      root.style.setProperty('--button-bg', this.currentTheme.buttonBg);
      root.style.setProperty('--button-text-color', this.currentTheme.buttonTextColor);
    },
  },
  created() {
    this.setTheme(localStorage.getItem('theme') || 'light'); // Initialize theme
  },
}
</script>

<template>
  <div class="container">
    <b-navbar class="" spaced>
      <template v-slot:brand>
        <b-navbar-item tag="router-link" :to="{ path: '/' }">
          <h1 class="title" style="font-family:Arial Black">Hammond</h1>
        </b-navbar-item>
      </template>
      <template v-slot:end>
        <NavBarRoutes :routes="persistentNavRoutes" />
        <NavBarRoutes v-if="loggedIn" :routes="loggedInNavRoutes" />
        <NavBarRoutes v-else :routes="loggedOutNavRoutes" />
        <b-navbar-dropdown v-if="loggedIn && isAdmin" label="Admin">
          <NavBarRoutes :routes="adminNavRoutes" />
        </b-navbar-dropdown>
        <!-- Theme Toggle Button -->
        <button class="btn btn-secondary ml-3" @click="toggleTheme">
          Toggle Theme
        </button>
      </template>
    </b-navbar>
  </div>
</template>

<style scoped>
/* Additional scoped styles for nav-bar */
</style>
