<script setup lang="ts">
import { ref, defineExpose } from "vue";
import Card from "../components/Card.vue"; // Vérifie ce chemin
import Modrinth from "../components/Modrinth.vue";
import Download from "../components/Download.vue";
/* Référence vers l'élément DOM */
const featuresRef = ref<HTMLElement | null>(null);

/* Exposer l'élément DOM pour permettre le scroll */
defineExpose({ featuresRef });
</script>

<template>
  <section ref="featuresRef" class="py-16 bg-[#1a2135]">
    <div class="container mx-auto px-6 text-center">
      <h2 class="text-3xl text-white font-bold">Nous rejoindre</h2>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-8 mt-8">
          <Card image="/images/tuto1.jpg" title="Étape 1">
              <template #description>
                  <h1>Télécharger et installer l'application Modrinth</h1>
                  <div class="flex justify-center mt-4">
                      <Modrinth />
                  </div>
              </template>
          </Card>
        
          <Card image="/images/tuto2.jpg" title="Étape 2">
              <template #description>
                  <h1>Connectez-vous à votre compte Microsoft</h1>
              </template>
          </Card>

          <Card image="/images/tuto3.jpg" title="Étape 3">
              <template #description>
                  <h1>
                      1/ Téléchargez le client moddé Raphter.io<br>
                      2/ Double-cliquez dessus pour lancer l'installation<br>
                      3/ Une fois l'installation terminée, cliquez sur JOUER !
                  </h1>
                  <div class="flex justify-center mt-4">
                      <Download />
                  </div>
              </template>
          </Card>
      </div>
    </div>
  </section>
</template>
