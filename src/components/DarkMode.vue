<template>
  <button @click="toggleDarkMode" class="bg-slate-100 dark:bg-slate-800">
    {{ `${isDarkMode ? "Light" : "Dark"}` }}
  </button>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  setup() {
    const isDarkMode = ref(false);

    const toggleDarkMode = () => {
      isDarkMode.value = !isDarkMode.value;
      document.body.classList.toggle("dark", isDarkMode.value);
    };

    onMounted(() => {
      if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
        toggleDarkMode();
      }
    });

    return { isDarkMode, toggleDarkMode };
  },
};
</script>