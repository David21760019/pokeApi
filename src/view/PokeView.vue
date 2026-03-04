<script setup>
import axios from 'axios'
import { ref, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'

const route = useRoute()
const router = useRouter()

const pokemon = ref(null)
const loading = ref(true)

const back = () => {
  router.push('/pokemon')
}

const getData = async () => {
  try {
    loading.value = true
    const { data } = await axios.get(
      `https://pokeapi.co/api/v2/pokemon/${route.params.name}`
    )
    pokemon.value = data
  } catch (error) {
    console.log(error)
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  getData()
})
</script>

<template>
  <div class="min-h-screen bg-gray-100 p-8">

    <div v-if="loading" class="text-center text-xl font-semibold">
      Cargando...
    </div>

    <div
      v-else-if="pokemon"
      class="max-w-5xl mx-auto bg-white rounded-3xl p-8 shadow-xl"
    >

      <!-- HEADER -->
      <div class="flex justify-between items-center mb-6">
        <h1 class="text-4xl font-bold capitalize">
          {{ pokemon.name }}
        </h1>
        <span class="text-gray-400 text-xl font-semibold">
          #{{ pokemon.id }}
        </span>
      </div>

      <!-- IMÁGENES -->
      <div class="flex flex-col md:flex-row justify-center items-center gap-8 mb-8">
        <img
          class="w-64 h-64 object-contain"
          :src="pokemon.sprites.other['official-artwork'].front_default"
        />
      </div>

      <!-- TIPOS -->
      <div class="flex justify-center gap-3 mb-6">
        <span
          v-for="type in pokemon.types"
          :key="type.type.name"
          class="px-4 py-1 bg-blue-100 text-blue-700 rounded-full capitalize font-semibold"
        >
          {{ type.type.name }}
        </span>
      </div>

      <!-- DATOS GENERALES -->
      <div class="grid grid-cols-2 md:grid-cols-4 gap-6 text-center mb-8">
        <div>
          <p class="font-semibold text-gray-600">Altura</p>
          <p>{{ pokemon.height / 10 }} m</p>
        </div>
        <div>
          <p class="font-semibold text-gray-600">Peso</p>
          <p>{{ pokemon.weight / 10 }} kg</p>
        </div>
        <div>
          <p class="font-semibold text-gray-600">Experiencia Base</p>
          <p>{{ pokemon.base_experience }}</p>
        </div>
        <div>
          <p class="font-semibold text-gray-600">Orden</p>
          <p>{{ pokemon.order }}</p>
        </div>
      </div>

      <!-- HABILIDADES -->
      <div class="mb-8">
        <h2 class="text-2xl font-bold mb-3">Habilidades</h2>
        <div class="flex flex-wrap gap-3">
          <span
            v-for="ability in pokemon.abilities"
            :key="ability.ability.name"
            class="px-3 py-1 bg-gray-200 rounded-lg capitalize"
          >
            {{ ability.ability.name }}
          </span>
        </div>
      </div>

      <!-- ESTADÍSTICAS -->
      <div class="mb-8">
        <h2 class="text-2xl font-bold mb-3">Estadísticas</h2>
        <div
          v-for="stat in pokemon.stats"
          :key="stat.stat.name"
          class="mb-4"
        >
          <div class="flex justify-between capitalize text-sm">
            <span>{{ stat.stat.name }}</span>
            <span>{{ stat.base_stat }}</span>
          </div>
          <div class="w-full bg-gray-200 rounded-full h-3">
            <div
              class="bg-red-500 h-3 rounded-full"
              :style="{ width: stat.base_stat + '%' }"
            ></div>
          </div>
        </div>
      </div>

      <!-- MOVIMIENTOS -->
      <div class="mb-8">
        <h2 class="text-2xl font-bold mb-3">Movimientos</h2>
        <div class="flex flex-wrap gap-2 max-h-40 overflow-y-auto">
          <span
            v-for="move in pokemon.moves.slice(0, 30)"
            :key="move.move.name"
            class="px-2 py-1 bg-gray-100 rounded text-sm capitalize"
          >
            {{ move.move.name }}
          </span>
        </div>
      </div>

      <!-- JUEGOS -->
      <div class="mb-8">
        <h2 class="text-2xl font-bold mb-3">Aparece en</h2>
        <div class="flex flex-wrap gap-2">
          <span
            v-for="game in pokemon.game_indices"
            :key="game.version.name"
            class="px-3 py-1 bg-yellow-100 rounded capitalize text-sm"
          >
            {{ game.version.name }}
          </span>
        </div>
      </div>

      <!-- BOTÓN VOLVER -->
      <div class="text-center mt-10">
        <button
          @click="back"
          class="px-6 py-2 bg-gray-800 text-white rounded-lg hover:bg-black transition"
        >
          Volver
        </button>
      </div>

    </div>

  </div>
</template>