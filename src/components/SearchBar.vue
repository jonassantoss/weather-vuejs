<script setup>
import { ref } from 'vue'

const emit = defineEmits(['search-city'])

const city = ref('')

function search() {
  if (city.value.trim()) {
    emit('search-city', city.value.trim())
    city.value = ''
  }
}
</script>

<template>
  <div class="flex mb-4">
    <input
      type="text"
      placeholder="Digite uma cidade..."
      class="flex-grow p-3 rounded-l-lg border-none focus:outline-none bg-white/50 placeholder-gray-700"
      v-model="city"
      @keyup.enter="search"
    />
    <button
      class="bg-blue-600 hover:bg-blue-700 text-white p-3 rounded-r-lg transition-colors duration-300"
      @click="search"
    >
      Buscar
    </button>
  </div>
</template>
