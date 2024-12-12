<script setup>
import Button from './Button.vue'; // Импортируем ваш компонент кнопки

</script>

<template>
  <div class="w-full pt-6">
    <h2 class="font-bold text-xl text-transparent bg-clip-text bg-gradient-to-r from-slate-300 to-slate-400 tracking-wider">
      Blockchains for search
    </h2>
    <p class="font-bold text-slate-600">Only Bitcoin is available for the trial version</p>
    <div class="grid grid-cols-5 py-4 gap-2">
      <Button text="BTC" />
      <Button text="BNB" />
      <Button text="SOL" />
      <Button text="AVAX" />
      <Button text="OP" />
      <Button text="ETH" />
      <Button text="TRON" />
      <Button text="LTC" />
      <Button text="POLYGON" />
      <Button text="DOGE" />
    </div>
  </div>
</template>
