<script setup>
import { ref, onMounted, watchEffect, onUnmounted } from 'vue'
import Button from './Button.vue'
import HwidModal from './UpgradeModal.vue'
import MultiSelectButtons from './MultiSelectButtons.vue'

const selectedCoins = ref({})
const isModalVisible = ref(false)
const subscriptionData = ref(null)
const daysLeft = ref(0)
const subscriptionMessage = ref('')

// Emits for passing data to parent
const emit = defineEmits(['update-selected-coins'])

// Handle selected coins update
function handleSelectedCoinsUpdate(updatedCoins) {
  selectedCoins.value = updatedCoins
  emit('update-selected-coins', updatedCoins)
  console.log('Selected Coins passed to parent:', updatedCoins)
}

// Open/Close modal
const openModal = () => {
  isModalVisible.value = true
}

const closeModal = () => {
  isModalVisible.value = false
}

// Retrieve subscription data from localStorage
const loadSubscriptionData = () => {
  const storedData = JSON.parse(localStorage.getItem('subscription'))

  if (storedData && localStorage.isSub) {
    subscriptionData.value = storedData
    const expirationDate = new Date(storedData)
    const today = new Date()

    // Calculate days remaining
    const timeDiff = expirationDate - today
    const remainingDays = Math.ceil(timeDiff / (1000 * 3600 * 24))

    if (remainingDays > 0) {
      daysLeft.value = remainingDays
      subscriptionMessage.value = `Only ${daysLeft.value} days left to upgrade!`
    } else {
      subscriptionMessage.value = 'Your subscription has expired!'
    }
  } else {
    subscriptionMessage.value = 'No active subscription.'
  }
}

// Watch for changes in subscription data
watchEffect(() => {
  const intervalId = setInterval(() => {
    loadSubscriptionData()
  }, 1000)

  // Очистка интервала при уничтожении компонента
  onUnmounted(() => {
    clearInterval(intervalId)
  })
})

onMounted(() => {
  loadSubscriptionData()
})
</script>

<template>
  <div class="w-80 py-6 px-4 flex items-center justify-start flex-col gap-y-4">
    <h1 class="font-bold text-3xl text-slate-100">
      Seed<span class="text-transparent bg-clip-text bg-gradient-to-br from-green-400 to-blue-600"
        >X</span
      >
    </h1>
    <div class="w-full h-0.5 bg-gradient-to-br from-green-400 to-blue-600"></div>
    <div class="not-draggable">
      <MultiSelectButtons @update-selected-coins="handleSelectedCoinsUpdate" />
    </div>
    <div class="flex flex-col justify-end h-full space-y-4">
      <!-- Dynamic subscription message -->
      <p class="text-xl text-transparent bg-clip-text bg-gradient-to-br from-green-400 to-blue-600">
        {{ subscriptionMessage }}
      </p>

      <Button class="w-full not-draggable" :text="'Upgrade'" :on-click="openModal" />
    </div>

    <hwid-modal v-if="isModalVisible" :show="isModalVisible" @close="closeModal" />
  </div>
</template>
