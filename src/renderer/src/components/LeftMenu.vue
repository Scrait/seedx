<script setup>
import Button from './Button.vue'
import { ref } from 'vue'
import HwidModal from './UpgradeModal.vue'

const isModalVisible = ref(false)

const openModal = () => {
  isModalVisible.value = true
}

const closeModal = () => {
  isModalVisible.value = false
}
</script>

<template>
  <div class="w-56 p-3 flex items-center justify-start flex-col gap-y-4">
    <h1 class="font-bold text-3xl text-slate-100">
      Seed<span class="text-transparent bg-clip-text bg-gradient-to-br from-green-400 to-blue-600"
        >X</span
      >
    </h1>
    <div class="w-full h-0.5 bg-gradient-to-br from-green-400 to-blue-600"></div>
    <Button :text="'Upgrade'" :on-click="openModal" />
    <hwid-modal v-if="isModalVisible" :show="isModalVisible" @close="closeModal" />
  </div>
</template>
