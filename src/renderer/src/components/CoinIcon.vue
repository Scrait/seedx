<template>
  <img :src="iconSrc" :alt="`${coin} icon`" />
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  coin: {
    type: String,
    required: true
  }
})

const availableIcons = {
  btc: new URL('../assets/icons/btc.svg', import.meta.url).href,
  eth: new URL('../assets/icons/eth.svg', import.meta.url).href,
  sol: new URL('../assets/icons/sol.svg', import.meta.url).href,
  avax: new URL('../assets/icons/avax.svg', import.meta.url).href,
  bnb: new URL('../assets/icons/bnb.svg', import.meta.url).href,
  doge: new URL('../assets/icons/doge.svg', import.meta.url).href,
  ltc: new URL('../assets/icons/ltc.svg', import.meta.url).href,
  op: new URL('../assets/icons/op.svg', import.meta.url).href,
  polygon: new URL('../assets/icons/polygon.svg', import.meta.url).href,
  tron: new URL('../assets/icons/tron.svg', import.meta.url).href
}

const iconSrc = computed(() => {
  const lowerCaseCoin = props.coin.toLowerCase()
  return (
    availableIcons[lowerCaseCoin] || new URL('@/assets/icons/default.svg', import.meta.url).href
  )
})
</script>
