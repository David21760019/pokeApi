<script setup>
  import { RouterLink, RouterView } from 'vue-router';
</script>

<template>
  <nav class="bg-gradient-to-r from-red-700 via-red-800 to-gray-900 shadow-xl">
    <div class="max-w-6xl mx-auto px-6">
      <div class="flex justify-between items-center h-16">

        <!-- Logo -->
        <div class="text-white font-extrabold text-2xl tracking-wider">
          🔴 PokéDex
        </div>

        <!-- Links -->
        <div class="flex gap-8">

          <RouterLink to="/" v-slot="{ isActive }">
            <span
              :class="[
                'font-semibold tracking-wide transition duration-300 cursor-pointer',
                isActive
                  ? 'text-white border-b-4 border-red-500 drop-shadow-[0_0_8px_rgba(255,0,0,0.9)]'
                  : 'text-gray-200 hover:text-white hover:drop-shadow-[0_0_6px_rgba(255,0,0,0.8)]'
              ]"
            >
              Home
            </span>
          </RouterLink>

          <RouterLink to="/about" v-slot="{ isActive }">
            <span
              :class="[
                'font-semibold tracking-wide transition duration-300 cursor-pointer',
                isActive
                  ? 'text-white border-b-4 border-red-500 drop-shadow-[0_0_8px_rgba(255,0,0,0.9)]'
                  : 'text-gray-200 hover:text-white hover:drop-shadow-[0_0_6px_rgba(255,0,0,0.8)]'
              ]"
            >
              About
            </span>
          </RouterLink>

          <RouterLink to="/posts" v-slot="{ isActive }">
            <span
              :class="[
                'font-semibold tracking-wide transition duration-300 cursor-pointer',
                isActive
                  ? 'text-white border-b-4 border-red-500 drop-shadow-[0_0_8px_rgba(255,0,0,0.9)]'
                  : 'text-gray-200 hover:text-white hover:drop-shadow-[0_0_6px_rgba(255,0,0,0.8)]'
              ]"
            >
              Posts
            </span>
          </RouterLink>

          <RouterLink to="/pokemon" v-slot="{ isActive }">
            <span
              :class="[
                'font-semibold tracking-wide transition duration-300 cursor-pointer',
                isActive
                  ? 'text-white border-b-4 border-red-500 drop-shadow-[0_0_8px_rgba(255,0,0,0.9)]'
                  : 'text-gray-200 hover:text-white hover:drop-shadow-[0_0_6px_rgba(255,0,0,0.8)]'
              ]"
            >
              Pokémon
            </span>
          </RouterLink>

        </div>
      </div>
    </div>
  </nav>

  <RouterView />
</template>


<style scoped></style>