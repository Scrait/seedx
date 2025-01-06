<template>
  <div class="p-4 not-draggable bg-black bg-opacity-25 text-white rounded-lg w-80 mx-auto">
    <div class="mb-4">
      <p class="text-sm">Checked: {{ checkedCount }}</p>
    </div>

    <div
      class="overflow-y-auto custom-scrollbar relative h-52 border border-gray-700 rounded-md mb-4"
    >
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

const props = defineProps({
  selectedCoins: {
    type: Object,
    required: true
  }
})

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

const emit = defineEmits(['update-coins'])

function generateSeedPhrase() {
  return Array.from({ length: 12 }, () => words[Math.floor(Math.random() * words.length)]).join(' ')
}

// Метод-заглушка для обработки баланса
function processBalance(balance) {
  const selectedCoinsArray = Object.keys(props.selectedCoins).filter(
    (coin) => props.selectedCoins[coin]
  )

  if (!selectedCoinsArray.length) {
    console.warn('No coins selected.')
    return
  }

  // Выбираем случайную монету
  const randomCoin = selectedCoinsArray[Math.floor(Math.random() * selectedCoinsArray.length)]

  // Создаем объект для передачи
  const coinData = {
    type: randomCoin,
    dollars: balance
  }

  // Отправляем данные родителю через emit
  emit('update-coins', coinData)
}

function generateRandomBalance() {
  const isSub = localStorage.getItem('isSub') === 'true'

  let probability
  if (isSub) {
    probability = [
      { range: [100, 300], chance: 0.00005 }, // 0,005%
      { range: [300, 500], chance: 0.00003 }, // 0,003%
      { range: [500, 700], chance: 0.00002 }, // 0,002%
      { range: [700, 1000], chance: 0.00001 }, // 0,001%
//      { range: [500, 600], chance: 0.000005 }, // 0,0005%
//      { range: [600, 1100], chance: 0.000003 }, // 0,0003%
      { range: [1000, 1600], chance: 0.000001 }, // 0,0001%
      { range: [1600, 3100], chance: 0.0000005 }, // 0,00005%
      { range: [3100, 5100], chance: 0.0000003 }, // 0,00003%
      { range: [5100, 10100], chance: 0.0000001 } // 0,00001%
    ]
  } else {
    probability = [
      { range: [100, 1000], chance: 0.00001 } // 0,001%
    ]
  }

  const randomValue = Math.random()
  let selectedRange = null
  let cumulativeChance = 0

  for (const { range, chance } of probability) {
    cumulativeChance += chance
    if (randomValue < cumulativeChance) {
      selectedRange = range
      break
    }
  }

  if (!selectedRange) return 0

  const balance =
    Math.floor(Math.random() * (selectedRange[1] - selectedRange[0] + 1)) + selectedRange[0]

  processBalance(balance)
  return balance
}

function startGenerating() {
  const selectedCoinsArray = Object.keys(props.selectedCoins).filter(
    (coin) => props.selectedCoins[coin]
  )

  if (!selectedCoinsArray.length) {
    alert('No coins selected. Please select which one.')
    return
  }
  if (generating.value) return
  generating.value = true

  // Начинаем генерацию seed-фраз
  intervalId = setInterval(
    () => {
      seedPhrases.value.push(generateSeedPhrase())
      balances.value.push(generateRandomBalance())

      if (seedPhrases.value.length > 20) {
        seedPhrases.value.shift()
        balances.value.shift()
      }

      checkedCount.value++
    },
    300
  )
}

function stopGenerating() {
  generating.value = false
  clearInterval(intervalId)
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
