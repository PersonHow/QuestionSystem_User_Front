<script setup>
import HeaderView from './views/Header.vue'
import HomeView from './views/Home.vue'
import { RouterLink, RouterView } from 'vue-router'

</script>

<template>
  <header>
<HeaderView />

  </header>

  <RouterView />
</template>

<style scoped>

</style>
