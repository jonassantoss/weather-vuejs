<script setup>
import { computed } from 'vue'

const props = defineProps({
  weather: {
    type: Object,
    required: true,
  },
})

const flagUrl = computed(() => {
  const countryCode = props.weather.sys.country.toLowerCase()
  return `https://flagcdn.com/w40/${countryCode}.png`
})
</script>

<template>
  <div
    v-if="weather"
    class="mt-6 text-gray-900"
    style="text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5)"
  >
    <div class="flex items-center justify-center gap-x-4">
      <h2 class="text-4xl font-bold">
        {{ weather.name }},
        {{ weather.sys.country }}
      </h2>
      <img :src="flagUrl" :alt="`Bandeira de ${weather.sys.country}`" class="h-8 rounded" />
    </div>

    <div class="flex items-center justify-center my-4">
      <img
        :src="`https://openweathermap.org/img/wn/${weather.weather[0].icon}@2x.png`"
        alt="Ícone do clima"
      />
      <p class="text-6xl font-light text-gray-50">{{ Math.round(weather.main.temp) }}°C</p>
    </div>

    <p class="text-xl font-bold text-gray-50 capitalize">
      {{ weather.weather[0].description }}
    </p>

    <div class="flex justify-around mt-6 bg-black/20 p-4 rounded-lg">
      <div>
        <p class="font-semibold text-slate-800">Sensação</p>
        <p class="text-gray-100">{{ Math.round(weather.main.feels_like) }}°C</p>
      </div>
      <div>
        <p class="font-semibold text-slate-800">Umidade</p>
        <p class="text-gray-100">{{ weather.main.humidity }}%</p>
      </div>
      <div>
        <p class="font-semibold text-slate-800">Vento</p>
        <p class="text-gray-100">{{ weather.wind.speed.toFixed(1) }}m/s</p>
      </div>
    </div>
  </div>
</template>
