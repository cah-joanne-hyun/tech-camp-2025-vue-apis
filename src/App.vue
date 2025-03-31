<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
    <div class="min-width-max-content">
      <nav>
        <RouterLink to="/">Search</RouterLink>
        <RouterLink to="/rated-movies">Rated Movies</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
.min-width-max-content {
  min-width: max-content;
}
</style>
