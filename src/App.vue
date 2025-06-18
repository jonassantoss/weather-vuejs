<script setup>
import { ref, onMounted, computed } from 'vue'
import SearchBar from './components/SearchBar.vue'
import CurrentWeather from './components/CurrentWeather.vue'
import WeatherForecast from './components/WeatherForecast.vue'

const weather = ref(null)
const forecast = ref(null)
const loading = ref(false)
const error = ref(null)

const apiKey = import.meta.env.VITE_OPEN_WEATHER_API_KEY

async function handleSearch(city) {
  loading.value = true
  error.value = null
  weather.value = null
  forecast.value = null

  try {
    const [weatherResponse, forecastResponse] = await Promise.all([
      fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${apiKey}&lang=pt_br`,
      ),

      fetch(
        `https://api.openweathermap.org/data/2.5/forecast?q=${city}&units=metric&appid=${apiKey}&lang=pt_br`,
      ),
    ])

    if (!weatherResponse.ok || !forecastResponse.ok) {
      if (weatherResponse.status === 404) {
        throw new Error('Cidade não encontrada')
      }
      throw new Error('Erro ao buscar dados do clima.')
    }

    weather.value = await weatherResponse.json()
    forecast.value = await forecastResponse.json()

    localStorage.setItem('lastCity', city)
  } catch (err) {
    error.value = err.message
  } finally {
    loading.value = false
  }
}

const dailyForecast = computed(() => {
  if (!forecast.value || !forecast.value.list) return []

  return forecast.value.list.filter((item) => item.dt_txt.includes('12:00:00'))
})

onMounted(() => {
  const lastCity = localStorage.getItem('lastCity')
  if (lastCity) {
    handleSearch(lastCity)
  }
})
</script>

<template>
  <div
    class="min-h-screen bg-gradient-to-br from-blue-400 to-indigo-600 flex flex-col items-center p-4"
  >
    <div class="w-full max-w-md bg-white/30 backdrop-blur-md rounded-lg shadow-lg p-6">
      <SearchBar @search-city="handleSearch" />

      <Transition name="fade">
        <div v-if="loading" class="text-center text-white mt-6">
          <p>Carregando...</p>
        </div>
      </Transition>

      <Transition name="fade">
        <div v-if="error" class="mt-4 bg-red-500/50 text-gray-400 p-3 rounded-lg">
          <p>{{ error }}</p>
        </div>
      </Transition>

      <div v-if="weather && dailyForecast.length > 0">
        <Transition name="fade">
          <CurrentWeather :weather="weather" />
        </Transition>
        <Transition name="fade" appear>
          <WeatherForecast :forecast="dailyForecast" />
        </Transition>
      </div>
    </div>
  </div>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
