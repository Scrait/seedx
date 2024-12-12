<template>
  <div class="absolute top-4 right-4 flex space-x-2 z-10 not-draggable">
    <button
      @click="minimizeWindow"
      class="w-8 h-8 text-gray-500 hover:text-white rounded-full flex items-center justify-center transition duration-300"
    >
      <span class="block w-5 h-[2px] bg-gray-300"></span>
    </button>

    <button
      @click="closeWindow"
      class="w-8 h-8 text-gray-500 hover:text-white rounded-full flex items-center justify-center transition duration-300"
    >
      <span class="block w-5 h-[2px] bg-gray-300 rotate-45 absolute"></span>
      <span class="block w-5 h-[2px] bg-gray-300 -rotate-45 absolute"></span>
    </button>
  </div>
</template>

<script setup>
  const minimizeWindow = () => {
    window.api.minimizeWindow();
  };

  const closeWindow = () => {
    window.api.closeWindow();
  };
</script>
