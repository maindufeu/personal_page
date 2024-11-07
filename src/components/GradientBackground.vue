<template>
  <div class="fixed inset-0 w-full h-full bg-gradient">
    <div class="absolute inset-0 bg-noise opacity-5"></div>
  </div>
</template>

<style scoped>
.bg-gradient {
  background: 
    linear-gradient(125deg, #1a1a2e 0%, #16213e 50%, #1a1a2e 100%),
    radial-gradient(circle at 20% 80%, #271080 0%, transparent 40%),
    radial-gradient(circle at 80% 20%, #16213e 0%, transparent 40%);
  background-blend-mode: screen;
}

.bg-noise {
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.65' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%' height='100%' filter='url(%23noise)'/%3E%3C/svg%3E");
  filter: contrast(150%) brightness(1000%);
}
</style>