<template>
    <div class="relative h-screen flex items-center justify-center text-center bg-transparent">
      <div class="absolute inset-0 flex flex-col items-center justify-center text-white px-6">
        
        <!-- Titre animé -->
        <h1 class="text-4xl md:text-6xl font-bold tracking-wide animate-fade-in">
          Bienvenue sur <span class="text-green-400">Raphter.io</span>
        </h1>
  
        <!-- Boutons (Download + Minecraft) -->
        <div class="mt-6 flex gap-4">
          
          <!-- Bouton Download -->
          <button
            @click="$emit('scroll-to-features')"
            class="flex items-center gap-3 px-8 py-3 bg-green-500 text-white text-lg font-semibold rounded-full shadow-lg transition-transform duration-300 hover:bg-green-600 hover:scale-105 cursor-pointer">
            
            <!-- Icône SVG -->
            <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960" width="24px" fill="white">
              <path d="M120-520v-320h320v320H120Zm0 400v-320h320v320H120Zm400-400v-320h320v320H520Zm0 400v-320h320v320H520Z"/>
            </svg>
  
            Download
          </button>
  
          <!-- Bouton Minecraft avec bulle au survol -->
          <div class="relative flex flex-col items-center">
            <button 
              @click="copyToClipboard"
              @mouseenter="showTooltip = true"
              @mouseleave="hideTooltip"
              class="px-6 py-3 bg-orange-500 text-white text-lg font-semibold rounded-full shadow-lg transition-transform duration-300 hover:bg-orange-600 hover:scale-105 cursor-pointer">
              minecraft.raphter.io
            </button>
  
            <!-- Bulle de copie SOUS le bouton -->
            <transition name="fade">
              <div v-if="showTooltip" class="absolute bottom-[-40px] bg-white text-black text-sm font-medium px-3 py-2 rounded-lg shadow-md">
                {{ tooltipText }}
                <!-- Flèche sous la bulle -->
                <div class="absolute w-3 h-3 bg-white rotate-45 -top-1 left-1/2 transform -translate-x-1/2"></div>
              </div>
            </transition>
          </div>
          
        </div>
      </div>
    </div>
  </template>
  
<script setup lang="ts">
  import { ref } from "vue";
  
  const showTooltip = ref(false);
  const tooltipText = ref("Copier");
  
  const copyToClipboard = () => {
    navigator.clipboard.writeText("minecraft.raphter.io").then(() => {
      tooltipText.value = "Copié !";
      showTooltip.value = true;
      setTimeout(() => {
        hideTooltip();
      }, 1000);
    });
  };
  
  const hideTooltip = () => {
    tooltipText.value = "Copier";
    showTooltip.value = false;
  };
</script>
  
<style>
  /* Animation d’apparition fluide */
  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(-10px); }
    to { opacity: 1; transform: translateY(0); }
  }
  
  .animate-fade-in {
    animation: fadeIn 1.2s ease-in-out;
  }
  
  /* Animation de la bulle */
  .fade-enter-active, .fade-leave-active {
    transition: opacity 0.2s ease-in-out, transform 0.2s ease-in-out;
  }
  
  .fade-enter-from, .fade-leave-to {
    opacity: 0;
    transform: translateY(5px); /* Monte doucement */
  }
</style>
  