<template>
  <div class="bg-[#101420] shadow-2xl p-6 rounded-xl text-center relative">
    <!-- 🔹 Titre en haut de la carte -->
    <h3 class="text-xl font-semibold mb-4">{{ title }}</h3>

    <!-- Image cliquable avec ratio 4:3 -->
    <img 
      :src="image" 
      :alt="title" 
      class="w-full aspect-[4/3] object-cover rounded-lg cursor-pointer transition duration-300 ease-in-out hover:opacity-75"
      @click="openModal"
    >

    <!-- Description personnalisée avec slot -->
    <p class="mt-2 text-xl text-white">
      <slot name="description"></slot>
    </p>

    <!-- Overlay (modal) avec fond flou -->
    <Transition name="fade">
      <div 
        v-if="isModalOpen" 
        class="fixed inset-0 flex justify-center items-center z-50 backdrop-blur-xl bg-white/30"
        @click.self="closeModal"
      >
        <div class="relative">
          <!-- Bouton de fermeture avec icône Material Symbols -->
          <button 
            @click="closeModal" 
            class="absolute top-2 right-2 text-gray-400 hover:text-gray-200 transition duration-300"
          >
            <span class="material-symbols-outlined text-2xl">
              close_fullscreen
            </span>
          </button>

          <!-- Image en grand format -->
          <img :src="image" :alt="title" class="max-w-full max-h-[80vh] rounded-lg drop-shadow-lg">
        </div>
      </div>
    </Transition>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from "vue";

defineProps({
  image: String,
  title: String
});

const isModalOpen = ref(false);

const openModal = () => {
  isModalOpen.value = true;
  document.addEventListener("keydown", closeOnEscape);
};

const closeModal = () => {
  isModalOpen.value = false;
  document.removeEventListener("keydown", closeOnEscape);
};

/* ✅ Ajout du type `KeyboardEvent` */
const closeOnEscape = (event: KeyboardEvent) => {
  if (event.key === "Escape") {
    closeModal();
  }
};

onMounted(() => {
  document.addEventListener("keydown", closeOnEscape);
});

onBeforeUnmount(() => {
  document.removeEventListener("keydown", closeOnEscape);
});
</script>

<style>
/* Animation smooth pour l'apparition de l'overlay */
.fade-enter-active {
  transition: opacity 0.7s ease-in-out;
}
.fade-enter-from {
  opacity: 0;
}
.fade-leave-to {
  opacity: 0;
}
</style>
