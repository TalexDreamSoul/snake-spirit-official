<script setup lang="ts">
import NavBar from '~/components/NavBar.vue'
</script>

<template>
  <div class="absolute left-0 top-0 h-full w-full flex flex-col">
    <NavBar class="top-0 w-full" />
    <div class="flex-1">
      <RouterView />
    </div>
  </div>
</template>
