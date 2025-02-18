<template>
  <div class="relative flex flex-col items-center">
    <button 
      :class="[
        'flex items-center gap-3 px-6 py-3 rounded-full font-semibold text-lg transition duration-300 shadow-lg',
        variantClasses,
        { 'cursor-pointer': !disabled, 'opacity-50 cursor-not-allowed': disabled }
      ]"
      :disabled="disabled"
      @click="handleClick"
      @mouseenter="handleHover"
      @mouseleave="handleLeave"
      ref="buttonRef"
    >
      <!-- Icône optionnelle -->
      <slot name="icon" />
      <!-- Texte du bouton -->
      <slot></slot>
    </button>

    <!-- Tooltip affiché lors du hover si clipboard est activé ou après la copie -->
    <transition name="fade">
      <div v-if="showTooltip" class="absolute bottom-[-40px] bg-white text-black text-sm font-medium px-3 py-2 rounded-lg shadow-md">
        {{ tooltipText }}
        <div class="absolute w-3 h-3 bg-white rotate-45 -top-1 left-1/2 transform -translate-x-1/2"></div>
      </div>
    </transition>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, defineProps } from "vue";

// Définition des types de variantes possibles pour le bouton
type ButtonVariants = "primary" | "secondary" | "danger" | "success";

// Définition des propriétés du bouton
const props = defineProps<{ 
  variant?: ButtonVariants; // Type de bouton (couleur, style)
  disabled?: boolean; // Désactivation du bouton
  clipboard?: boolean; // Active la copie du texte du bouton si true
  scrollTo?: string; // Sélecteur CSS de l'élément vers lequel scroller
}>();

// État pour afficher le tooltip après une copie
const showTooltip = ref(false);
const tooltipText = ref("Copier");

// Référence au bouton pour récupérer son texte dynamiquement
const buttonRef = ref<HTMLButtonElement | null>(null);

// Calcul des classes CSS selon la variante choisie
const variantClasses = computed(() => {
  return {
    primary: "bg-[#00A82D] text-white hover:bg-[#008C25]",
    secondary: "bg-orange-500 text-white hover:bg-orange-600",
    danger: "bg-red-500 text-white hover:bg-red-600",
    success: "bg-green-500 text-white hover:bg-green-600",
  }[props.variant || "primary"] || "bg-gray-500 text-white hover:bg-gray-600";
});

// Fonction exécutée au clic sur le bouton
const handleClick = async () => {
  if (props.clipboard && buttonRef.value) {
    const textToCopy = buttonRef.value.innerText.trim();
    await navigator.clipboard.writeText(textToCopy); // Copie le texte du bouton
    tooltipText.value = "Copié !";
    showTooltip.value = true;
    
    // Masque le tooltip après une seconde
    setTimeout(() => {
      showTooltip.value = false;
      tooltipText.value = "Copier";
    }, 1000);
  }
  if (props.scrollTo) {
    const element = document.querySelector(props.scrollTo);
    if (element) {
      element.scrollIntoView({ behavior: "smooth" });
    }
  }
};

// Fonction affichant "Copier" au survol si clipboard est activé
const handleHover = () => {
  if (props.clipboard) {
    tooltipText.value = "Copier";
    showTooltip.value = true;
  }
};

// Fonction masquant le tooltip au départ du survol
const handleLeave = () => {
  if (tooltipText.value !== "Copié !") {
    showTooltip.value = false;
  }
};
</script>

<style>
/* Animation d'affichage du tooltip */
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.2s ease-in-out, transform 0.2s ease-in-out;
}

.fade-enter-from, .fade-leave-to {
  opacity: 0;
  transform: translateY(5px);
}
</style>