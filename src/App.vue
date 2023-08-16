<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
    <div class="w-full h-16 bg-red-500 flex items-center justify-center">
      <nav class="space-x-10 font-bold text-white text-2xl">
        <RouterLink to="/">Corrective Action Demo</RouterLink>
        <RouterLink to="/create-project">Create Project Demo</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>
