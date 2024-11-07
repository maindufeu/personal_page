<script setup lang="ts">
import ChatBox from './components/ChatBox.vue'
import GradientBackground from './components/GradientBackground.vue'
import ParticleBackground from './components/ParticleBackground.vue'
</script>

<template>
  <div class="relative min-h-screen overflow-hidden">
    <GradientBackground />
    <ParticleBackground />
    
    <div class="relative z-10">
      <div class="container mx-auto px-4 py-8">
        <div class="text-center mb-8">
          <div class="w-48 h-48 mx-auto mb-6 rounded-full overflow-hidden shadow-xl ring-4 ring-blue-500/30">
            <img
              src="https://media.licdn.com/dms/image/v2/D5603AQHPtwLmK_fdrw/profile-displayphoto-shrink_400_400/profile-displayphoto-shrink_400_400/0/1730145057642?e=1736380800&v=beta&t=S71ubEz2ZVBEFsVNtfnuSplhcIyU0UY5qAIu2zEGqkA"
              alt="Edher Jimenez"
              class="w-full h-full object-cover"
            />
          </div>
          <h1 class="text-4xl font-bold text-white mb-2">Edher Jimenez</h1>
          <p class="text-xl text-blue-200">AI Magic Developer & Tech Enthusiast</p>
        </div>
        
        <ChatBox />
      </div>
    </div>
  </div>
</template>

<style>
body {
  margin: 0;
  background: #1a1a2e;
}</style>