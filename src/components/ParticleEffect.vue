<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';

interface Particle {
  x: number;
  y: number;
  size: number;
  speedY: number;
  wobbleSpeed: number;
  wobbleDistance: number;
  angle: number;
  texture: HTMLCanvasElement;
}

// Augmentation de 50% du nombre de particules
const PARTICLE_COUNT = 60;
const canvasRef = ref<HTMLCanvasElement | null>(null);
let ctx: CanvasRenderingContext2D | null = null;
let width = 0;
let height = 0;
let animationFrameId = 0;

// ------------------------------
// 1) GÉNÉRATION DE TEXTURES
// ------------------------------
// On crée plusieurs canvas offscreen pour chaque couleur et différentes tailles
// afin d'éviter de recalculer un radial gradient à chaque particule.
const colorDefs = [
  { name: 'white', color: 'rgba(255,255,255,1)' },
  { name: 'gold',  color: 'rgba(255,200,0,1)'   },
  { name: 'blue',  color: 'rgba(70,150,255,1)' }
];

// Réduction des tailles de moitié
const possibleSizes = [1, 1.5, 2];

interface TextureDef {
  canvas: HTMLCanvasElement;
  size: number;
}

const texturePool: TextureDef[] = [];

// Crée un offscreen canvas pour une couleur et une taille
function createTexture(size: number, color: string): HTMLCanvasElement {
  const offCanvas = document.createElement('canvas');
  const diameter = size * 4; // marge pour le dégradé
  offCanvas.width = diameter;
  offCanvas.height = diameter;

  const offCtx = offCanvas.getContext('2d');
  if (!offCtx) return offCanvas;

  // Gradient radial (centre = opaque, bords = transparent)
  const grad = offCtx.createRadialGradient(
    diameter / 2, diameter / 2, 0,
    diameter / 2, diameter / 2, diameter / 2
  );
  grad.addColorStop(0, color);
  grad.addColorStop(1, 'rgba(255,255,255,0)');

  offCtx.fillStyle = grad;
  offCtx.fillRect(0, 0, diameter, diameter);

  return offCanvas;
}

// On initialise toutes les textures au montage
function generateTextures() {
  texturePool.length = 0;
  for (const { color } of colorDefs) {
    for (const size of possibleSizes) {
      texturePool.push({
        canvas: createTexture(size, color),
        size
      });
    }
  }
}

// ------------------------------
// 2) PARTICULES & ANIMATION
// ------------------------------
const particles: Particle[] = [];

function createParticle(): Particle {
  // On choisit aléatoirement une texture dans notre pool
  const tex = texturePool[Math.floor(Math.random() * texturePool.length)];
  // Position initiale aléatoire en bas
  return {
    x: Math.random() * width,
    y: height + Math.random() * 20,
    size: tex.size,
    speedY: Math.random() * 1 + 0.05,
    wobbleSpeed: Math.random() * 0.03,
    wobbleDistance: Math.random() * 1 + 1,
    angle: Math.random() * Math.PI * 2,
    texture: tex.canvas
  };
}

function initParticles() {
  particles.length = 0;
  for (let i = 0; i < PARTICLE_COUNT; i++) {
    particles.push(createParticle());
  }
}

function animate() {
  if (!ctx) return;

  // Efface le canvas avant de redessiner
  ctx.clearRect(0, 0, width, height);

  // Dessine chaque particule
  for (const p of particles) {
    // Mise à jour position
    p.y -= p.speedY;
    p.angle += p.wobbleSpeed;
    p.x += Math.sin(p.angle) * p.wobbleDistance;

    // Dessin de la texture (centre sur la particule)
    const texSize = p.size * 4; // taille du offCanvas
    ctx.drawImage(
      p.texture,
      p.x - texSize / 2,
      p.y - texSize / 2
    );

    // Réinitialise si la particule sort par le haut
    if (p.y < -20) {
      Object.assign(p, createParticle());
    }
  }

  animationFrameId = requestAnimationFrame(animate);
}

// Ajuste la taille du canvas sur la fenêtre
function handleResize() {
  if (!canvasRef.value) return;
  width = canvasRef.value.width = window.innerWidth;
  height = canvasRef.value.height = window.innerHeight;
  initParticles();
}

// ------------------------------
// 3) HOOKS VUE
// ------------------------------
onMounted(() => {
  if (!canvasRef.value) return;
  ctx = canvasRef.value.getContext('2d');
  if (!ctx) return;

  // Génère une fois les textures
  generateTextures();

  // Mise à la bonne taille du canvas
  handleResize();
  window.addEventListener('resize', handleResize);

  // Initialise et lance l'animation
  initParticles();
  animate();
});

onUnmounted(() => {
  window.removeEventListener('resize', handleResize);
  cancelAnimationFrame(animationFrameId);
});
</script>

<template>
  <canvas ref="canvasRef" class="absolute inset-0 w-full h-full pointer-events-none" />
</template>
