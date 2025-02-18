<template>
  <div>
    <Header />
    <Hero @scroll-to-features="scrollToFeatures" />
    <Features ref="featuresSection" />
    <Footer />
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import Header from "../components/Header.vue";
import Hero from "../components/Hero.vue";
import Features from "../components/Features.vue";
import Footer from "../components/Footer.vue";

/* Référence correcte du composant Features */
const featuresSection = ref<{ featuresRef: HTMLElement } | null>(null);

const scrollToFeatures = () => {
  if (featuresSection.value?.featuresRef) {
    featuresSection.value.featuresRef.scrollIntoView({ behavior: "smooth" });
  }
};
</script>
