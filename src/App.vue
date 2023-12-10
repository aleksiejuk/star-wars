<script setup>
import { RouterLink, RouterView } from 'vue-router';
</script>

<template>
  <header>
    <div id="nav" class="navigation">
    <RouterLink to="/">Home</RouterLink>
    <RouterLink to="/about">About</RouterLink>
  </div>
  </header>
  <RouterView />
</template>

<style scoped>
.navigation {
  color: var( --color-dark-gray);
  margin: 1rem 2rem;
  position: absolute;
  top: 0;
  z-index: 30;
}
.navigation a {
  margin-right: 1.5rem;
}
</style>
