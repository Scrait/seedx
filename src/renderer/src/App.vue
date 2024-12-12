<template>
  <div class="relative rounded-md min-h-screen bg-gradient-to-tr from-[#101010] to-[#192630] draggable app">
    <WindowButtons />
    <div class="flex space-x-5 p-2">
      <LeftMenu />
      <BlockchainsSelect />
    </div>

  </div>
</template>

<script setup>
import WindowButtons from './components/WindowButtons.vue';
import LeftMenu from './components/LeftMenu.vue';
import BlockchainsSelect from './components/BlockchainsSelect.vue';
</script>
