<template>
  <nav class="w-full fixed top-0 left-0 bg-gray-900/80 backdrop-blur-md text-white py-4 shadow-md z-50">
    <div class="container mx-auto flex justify-between items-center px-6">
      <!-- Logo -->
      <!-- Ajout de @click pour scroller en haut -->
      <router-link
        to="/"
        class="text-2xl font-bold text-white hover:text-green-400 transition"
        @click="scrollToTop"
      >
        Raphter.io
      </router-link>

      <!-- Menu Desktop -->
      <ul class="hidden md:flex space-x-6">
        <li><a href="#" class="hover:text-green-400 transition">Accueil</a></li>
        <li><a href="#" class="hover:text-green-400 transition">Services</a></li>
        <li><a href="#" class="hover:text-green-400 transition">Contact</a></li>
      </ul>

      <!-- Bouton Connexion -->
      <button
        class="hidden md:flex items-center gap-2 bg-green-500 hover:bg-green-600 px-4 py-2 rounded-full text-white font-semibold shadow-md transition"
      >
        <!-- ...svg... -->
        Connexion
      </button>

      <!-- Bouton Burger (Mobile) -->
      <button @click="toggleMenu" class="md:hidden text-white focus:outline-none">
        <!-- ...svg... -->
      </button>
    </div>

    <!-- Menu Mobile -->
    <transition name="slide">
      <div
        v-if="isMenuOpen"
        class="md:hidden absolute top-full left-0 w-full bg-gray-900/90 backdrop-blur-md p-6 text-center shadow-md"
      >
        <ul class="space-y-4">
          <li><a href="#" class="block text-white hover:text-green-400 transition">Accueil</a></li>
          <li><a href="#" class="block text-white hover:text-green-400 transition">Services</a></li>
          <li><a href="#" class="block text-white hover:text-green-400 transition">Contact</a></li>
        </ul>
      </div>
    </transition>
  </nav>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const isMenuOpen = ref(false);

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value;
};

// Fonction pour scroller en haut de la page
const scrollToTop = () => {
  window.scrollTo({ top: 0, behavior: 'smooth' });
};
</script>

<style>
/* Animation du menu mobile */
.slide-enter-active, .slide-leave-active {
  transition: transform 0.3s ease-in-out, opacity 0.3s ease-in-out;
}
.slide-enter-from, .slide-leave-to {
  transform: translateY(-10px);
  opacity: 0;
}
</style>
