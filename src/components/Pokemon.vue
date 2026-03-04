<script setup>
import axios from 'axios'
import { ref, computed, onMounted } from 'vue'
import { RouterLink } from 'vue-router'

const pokemons = ref([])
const loading = ref(true)

const limit = 12
const currentPage = ref(1)
const totalPokemons = ref(0)

const totalPages = computed(() =>
  Math.ceil(totalPokemons.value / limit)
)

const getData = async () => {
  try {
    loading.value = true

    const offset = (currentPage.value - 1) * limit

    const { data } = await axios.get(
      `https://pokeapi.co/api/v2/pokemon?limit=${limit}&offset=${offset}`
    )

    totalPokemons.value = data.count

    pokemons.value = data.results.map((pokemon, index) => ({
      ...pokemon,
      id: offset + index + 1
    }))

  } catch (error) {
    console.log(error)
  } finally {
    loading.value = false
  }
}

const next = () => {
  if (currentPage.value < totalPages.value) {
    currentPage.value++
    getData()
  }
}

const previous = () => {
  if (currentPage.value > 1) {
    currentPage.value--
    getData()
  }
}

onMounted(() => {
  getData()
})
</script>

<template>
  <div class="min-h-screen bg-gray-100 p-8">

    <h1 class="text-4xl font-bold text-center mb-8">
      Pokédex 
    </h1>

    <div v-if="loading" class="text-center text-xl">
      Cargando...
    </div>

    <div v-else>

      <div class="grid gap-6  sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4">

        <RouterLink
          v-for="pokemon in pokemons"
          :key="pokemon.name"
          :to="`/pokemon/${pokemon.name}`"
          class="bg-white p-6 rounded-2xl shadow hover:shadow-l  transition text-center block"
        >
          <img
            class="mx-auto w-28"
            :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokemon.id}.png`"
            :alt="pokemon.name"
          />

          <h2 class="capitalize mt-3 font-semibold">
            {{ pokemon.name }}
          </h2>

          <p class="text-sm text-gray-400 mt-1">
            #{{ pokemon.id }}
          </p>

          <div class="mt-3 text-red-500 font-semibold text-sm">
            Ver detalles →
          </div>
        </RouterLink>

      </div>

      <div class="flex flex-col items-center gap-4 mt-10">

        <div class="text-gray-600">
          Página {{ currentPage }} de {{ totalPages }}
        </div>

        <div class="flex gap-4">
          <button
            @click="previous"
            :disabled="currentPage === 1"
            class="px-6 py-2 bg-gray-500 text-white rounded disabled:opacity-40"
          >
            ← Anterior
          </button>

          <button
            @click="next"
            :disabled="currentPage === totalPages"
            class="px-6 py-2 bg-red-500 text-white rounded disabled:opacity-40"
          >
            Siguiente →
          </button>
        </div>

      </div>

    </div>

  </div>
</template>