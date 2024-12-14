<template>
  <div class="relative min-h-screen bg-gradient-to-tr from-[#101010] to-[#192630] draggable app">
    <WindowButtons />
    <div class="flex space-x-5 p-2">
      <LeftMenu class="z-10" />
      <div>
        <BlockchainsSelect />
        <div class="flex space-x-5 p-2">
          <SeedGenerator />
          <SeedGenerator />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import WindowButtons from './components/WindowButtons.vue'
import LeftMenu from './components/LeftMenu.vue'
import BlockchainsSelect from './components/BlockchainsSelect.vue'
import SeedGenerator from './components/SeedGenerator.vue'
</script>
