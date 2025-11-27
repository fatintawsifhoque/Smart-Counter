<template>
  <div class="min-h-screen bg-linear-to-br from-purple-900 to-indigo-800 flex flex-col items-center justify-center p-4 text-white">
    <h1 class="text-3xl md:text-4xl font-bold mb-8 text-center">Counter App</h1>
    
    <!-- Counter Display -->
    <div 
      class="text-7xl md:text-8xl font-bold mb-8 transition-all duration-300"
      :class="count > 0 ? 'text-green-300' : count < 0 ? 'text-red-300' : 'text-white'"
    >
      {{ count }}
    </div>
    
    <!-- Control Buttons -->
    <div class="flex flex-col sm:flex-row gap-4 mb-8">
      <button 
        @click="decrement" 
        class="bg-red-600 hover:bg-red-700 text-white px-6 py-3 rounded-lg font-bold text-lg transition transform hover:scale-105"
      >
        -
      </button>
      
      <button 
        @click="reset" 
        class="bg-gray-600 hover:bg-gray-700 text-white px-6 py-3 rounded-lg font-bold text-lg transition transform hover:scale-105"
      >
        Reset
      </button>
      
      <button 
        @click="increment" 
        class="bg-green-600 hover:bg-green-700 text-white px-6 py-3 rounded-lg font-bold text-lg transition transform hover:scale-105"
      >
        +
      </button>
    </div>
    
    <!-- Step Selector -->
    <div class="mb-8">
      <label class="text-lg mr-3">Step Size:</label>
      <select 
  v-model.number="step" 
  class="bg-white/20 border border-white/30 rounded-lg p-2 text-black font-bold text-lg appearance-none focus:outline-none"
>
  <option :value="1">1</option>
  <option :value="5">5</option>
  <option :value="10">10</option>
  <option :value="25">25</option>
</select>
    </div>
    
    <!-- Keyboard Shortcut Hint -->
    <div class="text-center text-gray-300 mb-12">
      <p class="text-sm">Keyboard Shortcuts: ↑ / ↓ to count, R to reset</p>
    </div>
    
    <!-- Signature -->
    <div class="absolute bottom-6 text-center">
      <p class="text-lg md:text-xl">
        Made by 
        <a 
          href="https://github.com/fatintawsifhoque" 
          target="_blank"
          class="text-amber-300 font-bold text-xl md:text-2xl hover:text-amber-200 transition"
        >
          Fatin Tawsif Hoque
        </a>
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref, watch, onMounted, onUnmounted } from 'vue'

const count = ref(0)
const step = ref(1)

const increment = () => count.value += step.value
const decrement = () => count.value -= step.value
const reset = () => count.value = 0

// Keyboard shortcuts
const handleKeyDown = (e) => {
  if (e.key === 'ArrowUp') {
    increment()
  } else if (e.key === 'ArrowDown') {
    decrement()
  } else if (e.key.toLowerCase() === 'r') {
    reset()
  }
}

// Save to localStorage
watch(count, (newVal) => {
  localStorage.setItem('counterValue', JSON.stringify(newVal))
})

watch(step, (newVal) => {
  localStorage.setItem('counterStep', JSON.stringify(newVal))
})

// Load from localStorage
onMounted(() => {
  const savedCount = localStorage.getItem('counterValue')
  const savedStep = localStorage.getItem('counterStep')
  
  if (savedCount) count.value = JSON.parse(savedCount)
  if (savedStep) step.value = JSON.parse(savedStep)
  
  window.addEventListener('keydown', handleKeyDown)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeyDown)
})
</script>