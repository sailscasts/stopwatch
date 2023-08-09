<script setup>
import { ref, computed } from 'vue'
const startTime = ref(null)
const elapsedTime = ref(0)
const isRunning = ref(false)
const MILLISECONDS = 10
const toggleTimer = () => {
  if (isRunning.value) {
    clearInterval(startTime.value)
  } else {
    startTime.value = setInterval(() => {
      elapsedTime.value += MILLISECONDS
    }, MILLISECONDS)
  }
  isRunning.value = !isRunning.value
}

const resetTimer = () => {
  clearInterval(startTime.value)
  isRunning.value = false
  elapsedTime.value = 0
}

const formattedTime = computed(() => {
  const milliseconds = elapsedTime.value % 1000
  const seconds = Math.floor((elapsedTime.value / 1000) % 60)
  const minutes = Math.floor((elapsedTime.value / (1000 * 60)) % 60)
  const hours = Math.floor((elapsedTime.value / (1000 * 60 * 60)) % 24)

  return `${hours.toString().padStart(2, '0')}:${minutes
    .toString()
    .padStart(2, '0')}:${seconds.toString().padStart(2, '0')}.${milliseconds
    .toString()
    .slice(0, 2)
    .padStart(2, '0')}`
})
</script>

<template>
  <main
    class="flex flex-col items-center justify-center bg-gray-900 text-white p-8 shadow-md h-screen"
  >
    <p class="text-8xl font-bold font-mono">
      {{ formattedTime }}
    </p>
    <section class="flex justify-center space-x-4">
      <button
        class="px-4 py-2 rounded-lg border border-green-500 hover:bg-green-600 focus:outline-none transition"
        type="button"
        @click="toggleTimer"
      >
        {{ isRunning ? 'Stop' : 'Start' }}
      </button>
      <button
        class="px-4 py-2 rounded-lg bg-transparent border border-red-500 hover:bg-red-600 focus:outline-none transition disabled:cursor-not-allowed"
        type="button"
        @click="resetTimer"
        :disabled="isRunning"
      >
        Reset
      </button>
    </section>
  </main>
</template>
