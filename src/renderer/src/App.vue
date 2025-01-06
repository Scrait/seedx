<template>
  <div class="relative min-h-screen bg-gradient-to-tr from-[#101010] to-[#192630] draggable app">
    <WindowButtons class="z-50" />
    <div class="flex space-x-5 p-2">
      <LeftMenu class="z-10" @update-selected-coins="handleSelectedCoinsUpdate" />
      <div>
        <BlockchainsSelect />
        <div class="flex space-x-5 p-2">
          <SeedGenerator :selected-coins="selectedCoins" @update-coins="handleCoinUpdate" />
          <CoinList :coins="coins" />
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
import CoinList from './components/CoinList.vue'
import { ref, onMounted, watchEffect } from 'vue'

const coins = ref([])
const selectedCoins = ref({})

// Load coins from localStorage on component mount
onMounted(() => {
  const storedCoins = JSON.parse(localStorage.getItem('coins'))
  if (storedCoins) {
    coins.value = storedCoins
  }
})

// Update selected coins when the child emits the event
function handleSelectedCoinsUpdate(updatedCoins) {
  selectedCoins.value = updatedCoins
  console.log('Selected Coins:', updatedCoins)
}

// Handle updates from SeedGenerator to add new coins
function handleCoinUpdate(coinData) {
  console.log('Received coin data from SeedGenerator:', coinData)
  coins.value.push(coinData)

  // Save updated coins to localStorage
  localStorage.setItem('coins', JSON.stringify(coins.value))
}

// Save the coins to localStorage whenever they change
watchEffect(() => {
  if (coins.value.length > 0) {
    localStorage.setItem('coins', JSON.stringify(coins.value))
  }
})
</script>
