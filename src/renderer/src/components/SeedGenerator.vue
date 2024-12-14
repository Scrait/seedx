<template>
  <div class="p-4 not-draggable bg-black bg-opacity-25 text-white rounded-lg w-80 mx-auto">
    <div class="mb-4">
      <p class="text-sm">Checked: {{ checkedCount }}</p>
    </div>

    <div class="overflow-y-auto relative h-52 border border-gray-700 rounded-md mb-4">
      <transition-group name="fade" tag="ul" class="w-full">
        <li
          v-for="(phrase, index) in seedPhrases"
          :key="index"
          class="text-sm bg-gray-900 p-2 mb-1 rounded-md truncate"
        >
          {{ `Balance ${balances[index]}$ | ` }}Wallet check: {{ phrase }}
        </li>
      </transition-group>
    </div>

    <div class="flex justify-between">
      <CustomButton text="Start" :on-click="startGenerating" :active="!generating" />
      <CustomButton text="Stop" :on-click="stopGenerating" :active="generating" />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import CustomButton from './Button.vue'

const words = [
  'explain',
  'worry',
  'gori',
  'dish',
  'faith',
  'hover',
  'cricket',
  'disagree',
  'daring',
  'spy',
  'embrace',
  'history',
  'baby',
  'orphan',
  'pride',
  'gloom',
  'lobster',
  'toe',
  'upper',
  'click',
  'put',
  'grass',
  'sorry',
  'top',
  'rigid',
  'harvest',
  'transfer',
  'voice',
  'sand',
  'seat',
  'man',
  'apple',
  'ball',
  'house',
  'light',
  'winter',
  'dream',
  'fight',
  'moon',
  'star',
  'cold',
  'fire',
  'hope',
  'rain',
  'sun',
  'water',
  'peace',
  'stone',
  'mountain',
  'music',
  'road',
  'clock',
  'sound',
  'wind',
  'sky',
  'paper',
  'key',
  'tree',
  'heart',
  'love',
  'world',
  'life',
  'dream',
  'journey',
  'shadow',
  'cloud'
]

const checkedCount = ref(0)
const seedPhrases = ref([])
const balances = ref([])
const generating = ref(false)
let intervalId = null
let randomBalanceTimeout = null

function generateSeedPhrase() {
  return Array.from({ length: 12 }, () => words[Math.floor(Math.random() * words.length)]).join(' ')
}

// Метод-заглушка для обработки баланса
function processBalance(balance) {
  console.log(`Processing balance: ${balance}$`)
}

function generateRandomBalance() {
  randomBalanceTimeout = setTimeout(generateRandomBalance, Math.random() * 20000 + 20000)

  const balance = Math.floor(Math.random() * 5000) + 1000
  const randomIndex = Math.floor(Math.random() * seedPhrases.value.length)
  balances.value[randomIndex] = balance
  processBalance(balance)
}

function startGenerating() {
  if (generating.value) return
  generating.value = true

  // Начинаем генерацию seed-фраз
  intervalId = setInterval(() => {
    seedPhrases.value.push(generateSeedPhrase())
    balances.value.push(0)

    if (seedPhrases.value.length > 20) {
      seedPhrases.value.shift()
      balances.value.shift()
    }

    checkedCount.value++
  }, 100)

  // Начинаем генерацию случайного баланса
  generateRandomBalance()
}

function stopGenerating() {
  generating.value = false
  clearInterval(intervalId)
  clearTimeout(randomBalanceTimeout)
}
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition:
    opacity 0.5s,
    transform 0.5s;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(10px);
}

.truncate {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>
