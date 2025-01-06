<template>
  <div class="grid grid-cols-2 gap-2">
    <div v-for="(coin, index) in coins" :key="index" class="relative">
      <button
        :class="[
          'relative flex items-center justify-between w-full px-3 py-1.5 text-xs font-medium rounded-2xl focus:outline-none transition-colors duration-300',
          selectedCoins[coin]
            ? 'bg-gradient-to-br from-green-400 to-blue-600 text-white'
            : 'bg-black bg-opacity-25 text-white hover:bg-opacity-40',
          loadingCoins[coin] ? 'opacity-50 cursor-not-allowed' : ''
        ]"
        :disabled="loadingCoins[coin]"
        @click="handleCoinClick(coin)"
      >
        <CoinIcon class="w-4 h-4 me-2" :coin="coin" />

        <span class="flex-grow text-left overflow-hidden text-ellipsis whitespace-nowrap">{{
          coin
        }}</span>
        <span v-if="loadingCoins[coin]" class="absolute right-2">
          <svg
            class="animate-spin h-4 w-4 text-white"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
          >
            <circle
              class="opacity-25"
              cx="12"
              cy="12"
              r="10"
              stroke="currentColor"
              stroke-width="4"
            ></circle>
            <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8v8H4z"></path>
          </svg>
        </span>
        <span
          v-if="selectedCoins[coin] && !loadingCoins[coin]"
          class="absolute right-2 text-white text-lg"
          >✔️</span
        >
      </button>
    </div>
  </div>
</template>

<script setup>
import { reactive, onMounted, ref, watch } from 'vue'
import CoinIcon from './CoinIcon.vue'

const coins = ['BTC', 'BNB', 'SOL', 'AVAX', 'OP', 'ETH', 'TRON', 'LTC', 'POLYGON', 'DOGE']

const selectedCoins = reactive({})
const loadingCoins = reactive({})
const hwid = ref('')
const emit = defineEmits(['update-selected-coins'])
const socket = ref(null) // WebSocket

watch(
  () => selectedCoins,
  (newVal) => {
    emit('update-selected-coins', { ...newVal })
  },
  { deep: true }
)

const fetchHWID = async () => {
  hwid.value = await window.api.getHWID()
}

async function checkCoinAvailability(coin) {
  try {
    const response = await fetch('http://85.192.30.153:8080/api/keys/check-coin', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ coin, id: hwid.value })
    })

    if (!response.ok) {
      throw new Error('Ошибка сервера')
    }

    const data = await response.json()
    return data.allowed
  } catch (error) {
    console.error('Ошибка при запросе API:', error)
    alert('Ошибка при запросе к серверу. Попробуйте снова.')
    return false
  }
}

async function getAvailableCoinsFromServer() {
  try {
    const response = await fetch('http://85.192.30.153:8080/api/keys/get-or-create', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ id: hwid.value })
    })

    if (!response.ok) {
      throw new Error('Ошибка сервера')
    }

    const data = await response.json()

    localStorage.setItem('isSub', data.sub)
    localStorage.setItem('subscription', JSON.stringify(data.subscriptionExpirationDate))

    return data.coins
  } catch (error) {
    console.error('Ошибка при запросе API:', error)
    alert('Ошибка при загрузке данных. Попробуйте снова.')
    return []
  }
}

function saveSelectedCoins() {
  localStorage.setItem('selectedCoins', JSON.stringify(selectedCoins))
}

function loadSelectedCoins(availableCoins) {
  const savedCoins = JSON.parse(localStorage.getItem('selectedCoins') || '{}')

  for (const coin in savedCoins) {
    if (!availableCoins.includes(coin)) {
      delete savedCoins[coin]
    }
  }

  Object.assign(selectedCoins, savedCoins)
  saveSelectedCoins()
}

async function handleCoinClick(coin) {
  loadingCoins[coin] = true
  try {
    const isAllowed = await checkCoinAvailability(coin)
    if (isAllowed) {
      selectedCoins[coin] = !selectedCoins[coin]
      saveSelectedCoins()
    } else {
      alert(`You need to upgrade your subscription to access seed phrase search for ${coin}.`)
    }
  } catch (error) {
    console.error('Ошибка при проверке доступности монеты:', error)
  } finally {
    loadingCoins[coin] = false
  }
}

const setupWebSocket = () => {
  const url = `ws:/85.192.30.153:8080/ws/${hwid.value}`
  socket.value = new WebSocket(url)

  socket.value.onopen = () => console.log('Connected')
  socket.value.onerror = (error) => console.error('WebSocket error:', error)
  socket.value.onmessage = (event) => {
    const data = JSON.parse(event.data)
    localStorage.setItem('isSub', data.sub)
    localStorage.setItem('subscription', JSON.stringify(data.subscriptionExpirationDate))
    const availableCoins = data.coins
    const updatedSelectedCoins = { ...selectedCoins }

    // Проверяем и отключаем монеты, которых нет в списке с сервера
    for (const coin in updatedSelectedCoins) {
      if (!availableCoins.includes(coin)) {
        updatedSelectedCoins[coin] = false
      }
    }

    // Обновляем состояние выбранных монет и сохраняем
    Object.assign(selectedCoins, updatedSelectedCoins)
    saveSelectedCoins()
  }
}

onMounted(async () => {
  await fetchHWID()

  const availableCoins = await getAvailableCoinsFromServer()
  loadSelectedCoins(availableCoins)

  setupWebSocket()
})
</script>

<style scoped>
button {
  transition: all 0.3s ease;
}
button:hover {
  transform: scale(1.02);
}
button:disabled {
  pointer-events: none;
}
</style>
