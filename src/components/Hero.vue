<template>
  <div class="relative h-screen flex items-center justify-center text-center bg-transparent backdrop-blur-xs" style="background-image: linear-gradient(to top, rgba(0, 0, 0, 1), rgba(0, 0, 0, 0));">
    <div class="absolute inset-0 flex flex-col items-center justify-center text-white px-6">

      <ParticleEffect />
      
      <img src="/images/banner2.png" alt="Bannière" class="max-w-2xl h-auto mb-6" />

      <!-- Titre animé -->
      <h1 class="text-4xl md:text-6xl font-bold tracking-wide animate-fade-in">
        Bienvenue sur Raphter<span class="text-red-500">.io</span>
      </h1>

      <!-- Boutons (Download + Minecraft) -->
      <div class="mt-6 flex gap-4">
        
        <!-- Bouton Download -->
        <Button variant="primary" scrollTo="#features">
          <template #icon>
            <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960" width="24px" fill="white">
              <path d="M120-520v-320h320v320H120Zm0 400v-320h320v320H120Zm400-400v-320h320v320H520Zm0 400v-320h320v320H520Z"/>
            </svg>
          </template>
          Download
        </Button>

        <!-- Bouton Minecraft avec copie au clic -->
        <Button variant="secondary" :clipboard="true">
          minecraft.raphter.io
        </Button>

      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import Button from "../components/Button.vue";
import ParticleEffect from "../components/ParticleEffect.vue";
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
</style>
