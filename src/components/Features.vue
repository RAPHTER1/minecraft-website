<template>
    <section 
      ref="featuresRef" 
      class="py-16 bg-dark-700 scroll-mt-20"
      id="features"
    >
      <div class="container mx-auto px-6 text-center">
        <h2 class="text-5xl text-white font-bold mb-12">Comment nous rejoindre</h2>
  
        <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
          <TransitionGroup
            appear
            name="fade-up"
            class="grid grid-cols-1 md:grid-cols-3 gap-8"
          >
            <Card 
              v-for="(step, index) in steps"
              :key="step.title"
              :image="step.image"
              :title="`Étape ${index + 1}`"
            >
              <template #description>
                <div class="space-y-4">
                  <h3 class="text-xl font-medium">{{ step.title }}</h3>
                  <p class="text-white/80">{{ step.description }}</p>
                </div>
              </template>
              
              <template #button v-if="step.action">
                <div class="flex justify-center">
                  <component
                    :is="step.action.type === 'download' ? 'a' : Button"
                    v-bind="step.action.type === 'download' ? {
                      href: step.action.href,
                      download: step.action.download,
                      class: 'inline-flex items-center gap-3 px-6 py-3 rounded-full bg-[#00A82D] text-white font-semibold text-lg transition duration-300 hover:bg-[#008C25] shadow-lg cursor-pointer'
                    } : {
                      variant: step.action.variant,
                      ...step.action.props,
                      class: 'inline-flex items-center gap-3'
                    }"
                  >
                    <template v-if="step.action.type === 'download'">
                      <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960" width="24px" fill="white">
                        <path d="M160-80v-80h640v80H160Zm320-160L200-600h160v-280h240v280h160L480-240Z"/>
                      </svg>
                      Download modded client
                    </template>
                    <template v-else>
                      <svg v-if="step.action.icon" xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960" width="24px" fill="white">
                        <path d="M120-520v-320h320v320H120Zm0 400v-320h320v320H120Zm400-400v-320h320v320H520Zm0 400v-320h320v320H520Z"/>
                      </svg>
                      {{ step.action.label }}
                    </template>
                  </component>
                </div>
              </template>
            </Card>
          </TransitionGroup>
        </div>
      </div>
    </section>
  </template>
  
  <script setup lang="ts">
  import { ref, defineExpose } from "vue";
  import Card from "../components/Card.vue";
  import Button from "../components/Button.vue";
  
  const steps = [
    {
      title: "Télécharger Modrinth",
      description: "Installez l'application Modrinth pour gérer vos mods",
      image: "/images/tuto1.jpg",
      action: {
        variant: "primary",
        label: "Télécharger",
        icon: true,
        props: {
          "onClick": () => window.open("https://modrinth.com/app", "_blank")
        }
      }
    },
    {
      title: "Connexion Microsoft",
      description: "Connectez-vous avec votre compte Microsoft pour accéder au jeu",
      image: "/images/tuto2.jpg"
    },
    {
      title: "Installation du client",
      description: "Téléchargez et installez le client moddé Raphter.io",
      image: "/images/tuto3.jpg",
      action: {
        type: "download",
        href: "files/raphter_client_1.3.8.mrpack",
        download: "raphter_client_1.3.8.mrpack"
      }
    }
  ];
  
  const featuresRef = ref<HTMLElement | null>(null);
  defineExpose({ featuresRef });
  </script>
  
  <style>
  .fade-up-enter-active,
  .fade-up-leave-active {
    transition: all 0.5s ease;
  }
  
  .fade-up-enter-from {
    opacity: 0;
    transform: translateY(30px);
  }
  
  .fade-up-leave-to {
    opacity: 0;
    transform: translateY(-30px);
  }
  </style>