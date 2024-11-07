<script setup lang="ts">
import { onMounted, onBeforeUnmount, ref } from 'vue';

const canvas = ref<HTMLCanvasElement | null>(null);
let animationFrame: number;

interface Particle {
  x: number;
  y: number;
  size: number;
  speedX: number;
  speedY: number;
  opacity: number;
}

const particles: Particle[] = [];
const PARTICLE_COUNT = 50;

function createParticle(): Particle {
  return {
    x: Math.random() * window.innerWidth,
    y: Math.random() * window.innerHeight,
    size: Math.random() * 2 + 1,
    speedX: (Math.random() - 0.5) * 0.5,
    speedY: (Math.random() - 0.5) * 0.5,
    opacity: Math.random() * 0.5 + 0.2
  };
}

function initParticles() {
  for (let i = 0; i < PARTICLE_COUNT; i++) {
    particles.push(createParticle());
  }
}

function animate() {
  if (!canvas.value) return;
  const ctx = canvas.value.getContext('2d');
  if (!ctx) return;

  canvas.value.width = window.innerWidth;
  canvas.value.height = window.innerHeight;

  ctx.clearRect(0, 0, canvas.value.width, canvas.value.height);

  particles.forEach(particle => {
    particle.x += particle.speedX;
    particle.y += particle.speedY;

    if (particle.x < 0 || particle.x > canvas.value!.width) particle.speedX *= -1;
    if (particle.y < 0 || particle.y > canvas.value!.height) particle.speedY *= -1;

    ctx.beginPath();
    ctx.arc(particle.x, particle.y, particle.size, 0, Math.PI * 2);
    ctx.fillStyle = `rgba(147, 197, 253, ${particle.opacity})`;
    ctx.fill();
  });

  animationFrame = requestAnimationFrame(animate);
}

onMounted(() => {
  if (canvas.value) {
    canvas.value.width = window.innerWidth;
    canvas.value.height = window.innerHeight;
    initParticles();
    animate();
  }
});

onBeforeUnmount(() => {
  cancelAnimationFrame(animationFrame);
});
</script>

<template>
  <canvas
    ref="canvas"
    class="fixed top-0 left-0 w-full h-full pointer-events-none"
  ></canvas>
</template>