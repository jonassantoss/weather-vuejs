<script setup>
defineProps({
  forecast: {
    type: Array,
    required: true,
  },
})

function getDayOfWeek(dateString) {
  const date = new Date(dateString)
  return date.toLocaleString('pt-BR', { weekday: 'short' })
}
</script>

<template>
  <div class="mt-8">
    <h3
      class="text-2xl text-white font-semibold mb-4"
      style="text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5)"
    >
      Previsão para 5 dias
    </h3>

    <div class="grid grid-cols-5 gap-4">
      <div v-for="day in forecast" :key="day.dt" class="bg-white/20 p-3 rounded-lg text-center">
        <p class="font-bold text-slate-700">{{ getDayOfWeek(day.dt_txt) }}</p>
        <img
          :src="`https://openweathermap.org/img/wn/${day.weather[0].icon}.png`"
          alt="Ícone"
          class="mx-auto"
        />
        <p class="text-gray-600">{{ Math.round(day.main.temp) }}°C</p>
      </div>
    </div>
  </div>
</template>
