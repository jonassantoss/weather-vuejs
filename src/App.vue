<script setup>
import { ref } from 'vue'

const city = ref('')
const weather = ref(null)
const loading = ref(false)
const error = ref(null)

const apiKey = import.meta.env.VITE_OPEN_WEATHER_API_KEY

async function getWeather() {
  if (city.value === '') {
    error.value = 'Por favor, digite uma cidade.'
    return
  }

  loading.value = true
  error.value = null
  weather.value = null

  try {
    const url = `https://api.openweathermap.org/data/2.5/weather?q=${city.value}&units=metric&appid=${apiKey}&lang=pt_br`
    const response = await fetch(url)

    if (!response) {
      throw new Error('Cidade não encontrada')
    }

    const data = await response.json()
    weather.value = data
  } catch (err) {
    error.value = err.message
  } finally {
    loading.value = false
  }
}
</script>

<template>
  <div
    class="min-h-screen bg-gradient-to-br from-blue-400 to-indigo-600 flex flex-col items-center p-4"
  >
    <div class="w-full max-w-md bg-white/30 backdrop-blur-md rounded-lg shadow-lg p-6">
      <div class="flex mb-6">
        <input
          type="text"
          placeholder="Digite uma cidade..."
          class="flex-grow p-3 rounded-l-lg border-none focus:outline-none bg-white/50 placeholder-gray-700"
          v-model="city"
          @keyup.enter="getWeather"
        />
        <button
          class="bg-blue-600 hover:bg-blue-700 text-white p-3 rounded-r-lg transition-colors"
          @click="getWeather"
        >
          Buscar
        </button>
      </div>

      <div class="text-center text-gray-700">
        <div v-if="error" class="mt-4 bg-red-500 text-white p-3 rounded-lg">
          <p>Carregando...</p>
        </div>

        <div v-if="error" class="mt-4 bg-red-500/50 text-gray-400 p-3 rounded-lg">
          <p>{{ error }}</p>
        </div>

        <div v-if="weather" class="mt-6 text-gray-900 text-shadow-lg">
          <h2 class="text-4xl font-bold">
            {{ weather.name }},
            {{ weather.sys.country }}
          </h2>

          <div class="flex items-center justify-center my-4">
            <img
              :src="`https://openweathermap.org/img/wn/${weather.weather[0].icon}@2x.png`"
              alt="Ícone do clima"
            />
            <p class="text-6xl font-light">{{ Math.round(weather.main.temp) }}°C</p>
          </div>

          <p class="text-xl capitalize">
            {{ weather.weather[0].description }}
          </p>

          <div class="flex justify-around mt-6 bg-black/20 p-4 rounded-lg">
            <div>
              <p class="font-semibold">Sensação</p>
              <p>{{ Math.round(weather.main.feels_like) }}°C</p>
            </div>
            <div>
              <p class="font-semibold">Umidade</p>
              <p>{{ weather.main.humidity }}%</p>
            </div>
            <div>
              <p class="font-semibold">Vento</p>
              <p>{{ weather.wind.speed }}m/s</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
