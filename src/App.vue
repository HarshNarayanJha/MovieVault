<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <div class="wrapper">
    <!-- <p class="nav-title">Movie Vault</p> -->
    <!-- <img class="logo" src="https://fav.farm/🎬" alt="icon" width="35"> -->
    <nav>
      <RouterLink to="/">Home</RouterLink>
      <RouterLink to="/about">About</RouterLink>
    </nav>
  </div>

  <RouterView />
</template>

<style scoped>

.wrapper {
  display: flex;
}

.nav-title {
  font-size: 1.1rem;
  min-width: 100px;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}
</style>
