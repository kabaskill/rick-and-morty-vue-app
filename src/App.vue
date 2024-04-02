<script setup>
import { ref, watch } from "vue";
import Pages from "./components/Pages.vue";
import { Progress } from "@/components/ui/progress";
import DarkMode from "./components/DarkMode.vue";

const artificalDelay = 800;
const progress = ref(0);

const chars = ref(null);
const charPageNumber = ref(1);

const eps = ref(null);
const epPageNumber = ref(1);

const baseUrl = "https://rickandmortyapi.com/api";

fetchChars();
updateProgress();

watch(charPageNumber, fetchChars);


function updateProgress() {
  progress.value = 0;
  const frequency = artificalDelay / 10;

  const interval = setInterval(() => {
    if (progress.value < 100) {
      progress.value += 10;
    }
    if (progress.value >= 100) {
      clearInterval(interval);
    }
  }, frequency);
}

async function fetchChars(page = 1) {
  chars.value = null;
  charPageNumber.value = page;

  setTimeout(async () => {
    const responseChars = await fetch(`${baseUrl}/character?page=${charPageNumber.value}`);
    const responseEps = await fetch(`${baseUrl}/episode`);

    chars.value = await responseChars.json();
    eps.value = await responseEps.json();
  }, artificalDelay);
  updateProgress();
}
</script>

<template>
  <header class="flex justify-between">
    <h1 class="green text-2xl">Rick and Morty App</h1>
    <nav>
      <a href="" class="self-center">Home</a>
      <a href="" class="self-center">About</a>
      <DarkMode />
    </nav>
  </header>

  <main>
    <div
      v-if="chars"
      class="p-4 my-4 size-full dark:bg-gray-900 bg-slate-200 border flex flex-col rounded-xl"
    >
      <h2 class="text-xl">All Characters</h2>
      <ul v-if="chars" class="grid grid-cols-7 my-4">
        <a
          :href="`https://rickandmortyapi.com/api/character/${character.id}`"
          class="size-full p-2"
          v-for="character in chars.results"
          :key="character.id"
        >
          <img :src="character.image" :alt="character.name" />
          <p>{{ character.name }}</p>
        </a>
      </ul>

      <Pages :totalPages="chars.info.pages ?? 0" class="self-center" @page-changed="fetchChars" />
    </div>
    <Progress :modelValue="progress" v-else />

    <!-- <div class="p-2 my-4 bg-gray-900 border flex flex-col">
      <h2 class="text-xl">All Episodes</h2>
      <ul v-if="eps" class="grid grid-cols-4 my-4">
        <a
          :href="`https://rickandmortyapi.com/api/episode/${episode.id}`"
          class="size-full p-2"
          v-for="episode in eps.results"
          :key="episode.id"
        >
          <p>{{ episode.name }}</p>
        </a>
      </ul>
    </div> -->
  </main>
</template>

<style scoped>
nav {
  display: flex;
  gap: 1rem;
}

ul {
  list-style: none;
}
</style>
