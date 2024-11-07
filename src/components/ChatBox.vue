<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { OpenAI } from '@langchain/openai';
import { RetrievalQAChain } from 'langchain/chains';
import { MemoryVectorStore } from 'langchain/vectorstores/memory';
import { OpenAIEmbeddings } from '@langchain/openai';

const question = ref('');
const answer = ref('');
const loading = ref(false);
const error = ref('');
const isInitialized = ref(false);

let chain: RetrievalQAChain | null = null;

// Data about Edher
const data = [
  "Edher Jimenez is an AI developer",
  "Edher ganó el premio Reto Banxico por sus trabajos sobre el impacto de la política monetaria en el Covid 19",
  "Edher ha contribuido a Kaggle en diferentes concursos",
  "Edher ha trabajado con diferentes marcas y consultoras internacionales",
  "Edher lleva más de 7 años de experiencia haciendo análisis y ciencia de datos así como proyectos de IA",
  "Edher ha desarrollado más de 50 proyectos de IA",
  "Edher ha usado los siguientes lenguajes de programación: Python, R, SQL, Java, Javascript, HTML, CSS, C++",
  "Edher ha trabajado haciendo aplicaciones de react, vue, svelte, nextjs",
  "Edher tiene 31 años vive en cdmx y actualmente está a cargo del área de Innovación y laboratorios de IA en Minsait",
  "A Edher le gustan los robots, y su hobbie es hacer jabones y cocinar con sus esposa",
  "Edher estudió Economía y Psicología en la UNAM con un enfoque especial hacia la complejidad, representaciones sociales psicometría y análisis del comportamiento",
  // Add more facts about Edher
];

async function initializeAI() {
  const apiKey = import.meta.env.VITE_OPENAI_API_KEY;
  
  if (!apiKey) {
    error.value = 'OpenAI API key not configured. Please add it to your .env file.';
    return;
  }

  try {
    const model = new OpenAI({ 
      openAIApiKey: apiKey,
      temperature: 0.5 
    });

    const vectorStore = new MemoryVectorStore(
      new OpenAIEmbeddings({ openAIApiKey: apiKey })
    );

    await vectorStore.addDocuments(data.map(text => ({ pageContent: text })));
    
    chain = RetrievalQAChain.fromLLM(
      model,
      vectorStore.asRetriever()
    );

    isInitialized.value = true;
    error.value = '';
  } catch (e) {
    error.value = 'Failed to initialize AI system. Please check your configuration.';
    console.error('Initialization error:', e);
  }
}

async function askQuestion() {
  if (!question.value.trim() || !isInitialized.value || !chain) return;
  
  loading.value = true;
  error.value = '';
  
  try {
    const response = await chain.call({
      query: question.value
    });
    answer.value = response.text;
  } catch (e) {
    error.value = 'Failed to process your question. Please try again.';
    console.error('Query error:', e);
  } finally {
    loading.value = false;
  }
}

onMounted(() => {
  initializeAI();
});
</script>

<template>
  <div class="w-full max-w-2xl mx-auto mt-8 p-6 bg-gray-900/50 backdrop-blur-lg rounded-lg shadow-lg border border-blue-500/20">
    <div v-if="error" class="mb-4 p-4 bg-red-900/50 text-red-200 rounded-lg border border-red-500/20">
      {{ error }}
    </div>
    
    <div class="flex flex-col space-y-4">
      <input
        v-model="question"
        type="text"
        placeholder="Ask me anything about Edher..."
        class="w-full px-4 py-2 bg-gray-800/50 text-white border border-blue-500/20 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 placeholder-gray-400"
        @keyup.enter="askQuestion"
        :disabled="!isInitialized"
      />
      <button
        @click="askQuestion"
        :disabled="loading || !isInitialized"
        class="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 disabled:bg-gray-600 disabled:cursor-not-allowed transition-colors duration-200"
      >
        {{ loading ? 'Thinking...' : 'Ask' }}
      </button>
      <div v-if="answer" class="mt-4 p-4 bg-gray-800/50 rounded-lg border border-blue-500/20">
        <p class="text-blue-100">{{ answer }}</p>
      </div>
    </div>
  </div>
</template>