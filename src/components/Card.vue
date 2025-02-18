<template>
  <div class="bg-dark-900 shadow-2xl p-6 rounded-xl text-center relative flex flex-col h-full justify-between">
    <!-- Conteneur supérieur (titre, image) -->
    <div>
      <h3 class="text-xl text-white font-semibold mb-4">{{ title }}</h3>

      <img 
        :src="image" 
        :alt="title" 
        class="w-full aspect-[4/3] object-cover rounded-lg cursor-pointer transition duration-300 ease-in-out hover:opacity-75"
        @click="openModal"
      >
    </div>

    <!-- Conteneur intermédiaire (description) -->
    <div class="flex-grow">
      <p class="mt-2 text-xl text-white">
        <slot name="description"></slot>
      </p>
    </div>

    <!-- Bouton aligné en bas -->
    <div class="mt-auto pt-4">
      <slot name="button"></slot>
    </div>

    <!-- Overlay (modal) avec fond flou -->
    <Transition name="fade">
      <div 
        v-if="isModalOpen" 
        class="fixed inset-0 flex justify-center items-center z-50 backdrop-blur-xl bg-white/30"
        @click.self="closeModal"
      >
        <div class="relative">
          <Button variant="danger" class="absolute mb-5 right-2" @click="closeModal">
            <template #icon>
              <span class="material-symbols-outlined text-2xl">close</span>
            </template>
          </Button>

          <img :src="image" :alt="title" class="max-w-full max-h-[80vh] rounded-lg drop-shadow-lg">
        </div>
      </div>
    </Transition>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import Button from "../components/Button.vue";

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

const closeOnEscape = (event: KeyboardEvent) => {
  if (event.key === "Escape") {
    closeModal();
  }
};
</script>

<style>
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
