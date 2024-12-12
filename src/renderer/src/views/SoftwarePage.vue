<template>
  <div class="flex items-center justify-center min-h-screen">
    <GradientButton text="Нажми меня" @click="handleClick" />
  </div>
</template>

<script setup>
import GradientButton from '../components/Button.vue';

function handleClick() {
  alert('Кнопка нажата!');
}
</script>

<style scoped>
</style>
