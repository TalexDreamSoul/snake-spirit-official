<script setup lang="ts">
import NavBar from '~/components/NavBar.vue'
</script>

<template>
  <div class="absolute left-0 top-0 h-full w-full flex flex-col">
    <NavBar class="w-ful top-0" />
    <div class="flex-1">
      <RouterView />
    </div>
  </div>
</template>
