<template>
  <div class="absolute top-4 right-4 flex space-x-2 z-10 not-draggable">
    <MinimizeButton @click="minimizeWindow" />
    <CloseButton @click="closeWindow" />
  </div>
</template>

<script setup>
import CloseButton from './buttons/CloseButton.vue'
import MinimizeButton from './buttons/MinimizeButton.vue'

const minimizeWindow = () => {
  window.api.minimizeWindow()
}

const closeWindow = () => {
  window.api.closeWindow()
}
</script>
