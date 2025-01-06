<template>
  <div class="p-4 not-draggable bg-black bg-opacity-25 text-white rounded-lg w-80 mx-auto">
    <div class="mb-4">
      <p class="text-sm font-semibold text-gray-300">Found: {{ coins.length }}</p>
    </div>

    <div
      class="overflow-y-auto custom-scrollbar relative h-52 border border-gray-700 rounded-md mb-4"
    >
      <ul class="p-1.5 space-y-1.5">
        <li
          v-for="(coin, index) in coins"
          :key="index"
          class="flex justify-between items-center p-2 rounded-2xl transition-transform duration-300 border-2 cursor-pointer"
          :class="[getBorderColor(coin.type), isSelected(index) ? 'bg-gradient-to-r from-gray-700 via-gray-800 to-gray-700 transition duration-300 ease-in-out' : 'transition duration-300 ease-in-out']"
          @click="toggleSelection(index)"
        >
          <CoinIcon class="w-6 h-6" :coin="coin.type" />

          <div class="flex-1 ml-2">
            <p class="text-sm font-medium text-gray-300">{{ coin.type }}</p>
          </div>

          <span class="text-sm font-semibold text-green-400">${{ coin.dollars }}</span>
        </li>
      </ul>
    </div>

    <CustomButton
      class="w-full"
      :class="{ 'cursor-not-allowed': isSubDisabled }"
      :on-click="handleWithdraw"
      text="Withdraw"
    />

    <PaymentModal
      v-if="isModalVisible"
      class="z-10"
      :total-amount="totalAmount"
      :show="isModalVisible"
      @close="closeModal"
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import CustomButton from './Button.vue'
import PaymentModal from './PaymentModal.vue'
import CoinIcon from './CoinIcon.vue'

const props = defineProps({
  coins: {
    type: Array,
    default: () => []
  }
})

const selectedIndexes = ref([])
const isSubDisabled = ref(true)

const getBorderColor = (type) => {
  const colors = {
    BTC: 'border-orange-400',
    ETH: 'border-gray-400',
    SOL: 'border-cyan-400',
    BNB: 'border-yellow-400',
    AVAX: 'border-red-400',
    OP: 'border-pink-400',
    TRON: 'border-red-600',
    LTC: 'border-gray-300',
    POLYGON: 'border-purple-400',
    DOGE: 'border-yellow-300'
  }
  return colors[type] || 'border-gray-400'
}

const toggleSelection = (index) => {
  if (selectedIndexes.value.includes(index)) {
    selectedIndexes.value = selectedIndexes.value.filter((i) => i !== index)
  } else {
    selectedIndexes.value.push(index)
  }
}

const isSelected = (index) => selectedIndexes.value.includes(index)

const isModalVisible = ref(false)
const totalAmount = ref(0)

const openModal = () => {
  isModalVisible.value = true
}

const closeModal = () => {
  isModalVisible.value = false
}

const handleWithdraw = () => {
  if (localStorage.getItem('isSub') === 'false') return
  const selectedCoins = selectedIndexes.value.map((index) => props.coins[index])
  totalAmount.value = selectedCoins.reduce((sum, coin) => sum + coin.dollars, 0)
  console.log('Selected Coins:', selectedCoins)
  console.log('Total Amount in USD:', totalAmount.value)
  openModal()
}

onMounted(() => {
  // Ensure localStorage is accessed only on the client-side
  if (typeof window !== 'undefined' && localStorage.getItem('isSub') === 'true') {
    isSubDisabled.value = false
  }
})
</script>
