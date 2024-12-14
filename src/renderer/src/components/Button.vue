<template>
  <button
    @click="onClick"
    :class="[
      'not-draggable relative inline-flex items-center justify-center p-0.5 mb-2 me-2 overflow-hidden text-sm font-medium text-gray-900 rounded-2xl group focus:outline-none',
      active
        ? 'bg-gradient-to-br from-green-400 to-blue-600 text-white'
        : 'bg-gradient-to-br from-green-400 to-blue-600 group-hover:from-green-400 group-hover:to-blue-600 hover:text-white dark:text-white focus:ring-green-200 dark:focus:ring-green-800',
    ]"
  >
    <span
      :class="[
        'relative w-full px-5 py-2.5 transition-all ease-in duration-75 rounded-xl',
        active
          ? 'bg-opacity-0 text-white'
          : 'bg-white dark:bg-gray-900 group-hover:bg-opacity-0',
      ]"
    >
      {{ text }}
    </span>
  </button>
</template>


<script setup>
defineProps({
  text: {
    type: String,
    required: true
  },
  onClick: {
    type: Function,
    required: false
  },
  active: {
    type: Boolean,
    default: false
  }
})
</script>
