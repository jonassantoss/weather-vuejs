<script setup></script>

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
        />
        <button class="bg-blue-600 hover:bg-blue-700 text-white p-3 rounded-r-lg transition-colors">
          Buscar
        </button>
      </div>

      <div class="text-center text-gray-700">
        <p>Aguardando sua busca...</p>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
