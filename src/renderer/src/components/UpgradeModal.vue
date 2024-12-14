<script setup>
import { onMounted, ref } from 'vue'
import Button from './Button.vue'
import CloseButton from './buttons/CloseButton.vue'
import { vOnClickOutside } from '@vueuse/components'

const hwid = ref('')

defineProps({
  show: {
    type: Boolean,
    required: true
  }
})

const emit = defineEmits(['close'])

const fetchHWID = async () => {
  hwid.value = await window.api.getHWID()
}

const copyHWID = () => {
  navigator.clipboard.writeText(hwid.value).then(() => {
    alert('HWID скопирован!')
  })
}

const contactTelegram = () => {
  window.open('https://t.me/itheadteam', '_blank')
}

const closeModal = () => {
  emit('close')
}

onMounted(fetchHWID)
</script>

<template>
  <div
    v-if="show"
    class="not-draggable fixed inset-0 bg-black bg-opacity-50 flex justify-center items-center"
  >
    <div v-on-click-outside="closeModal" class="bg-[#0d161d] rounded-lg p-6 w-96 relative">
      <close-button class="absolute top-2 right-2" @click="closeModal" />
      <h2 class="text-white text-lg font-bold mb-4">Upgrade Required</h2>
      <p class="text-gray-300 mb-4 text-sm">
        To gain access to additional blockchain features, please copy your HWID and contact us via
        Telegram.
      </p>
      <div class="flex justify-between items-center">
        <Button :text="'Copy HWID'" :on-click="copyHWID" />
        <Button :text="'Contact via Telegram'" :on-click="contactTelegram" />
      </div>
    </div>
  </div>
</template>
