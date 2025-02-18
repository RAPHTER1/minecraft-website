<template>
  <nav class="w-full fixed top-0 left-0 bg-gray-900/80 backdrop-blur-md text-white py-4 shadow-md z-50">
    <div class="container mx-auto flex justify-between items-center px-6">
      <!-- Logo + Titre (Alignement avec Flexbox) -->
      <router-link
        to="/"
        class="flex items-center text-2xl font-bold text-white hover:text-green-400 transition"
        @click="scrollToTop"
      >
        <img
          src="/images/company-logo.png"
          alt="Company Logo"
          class="h-8 w-8 mr-2"
        />
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
        <svg
          xmlns="http://www.w3.org/2000/svg"
          height="20px"
          viewBox="0 -960 960 960"
          width="20px"
          fill="white"
        >
          <path d="M480-80q-66 0-123.5-25T257-173q-43-43-68-100.5T164-397q0-65 25-122.5T257-620q43-43 100.5-68T480-713q40 0 78 8.5t74 24.5l-52 52q-25-9-51-13.5T480-646q-108 0-184 75.5T220-386q0 107 75.5 183.5T480-127q108 0 183-76t75-183q0-27-4.5-53.5T721-493l52-52q17 36 25 74.5t8 78.5q0 66-25 123.5T703-173q-43 43-100.5 68T480-80Zm0-240-57-57 101-103H280v-80h244l-101-103 57-57 184 184-184 184Z"/>
        </svg>
        Connexion
      </button>
  
      <!-- Bouton Burger (Mobile) -->
      <button @click="toggleMenu" class="md:hidden text-white focus:outline-none">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="w-8 h-8"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7" />
        </svg>
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

// Pour revenir en haut de la page
const scrollToTop = () => {
  window.scrollTo({ top: 0, behavior: 'smooth' });
};
</script>

<style>
/* Animation du menu mobile */
.slide-enter-active,
.slide-leave-active {
  transition: transform 0.3s ease-in-out, opacity 0.3s ease-in-out;
}
.slide-enter-from,
.slide-leave-to {
  transform: translateY(-10px);
  opacity: 0;
}
</style>
