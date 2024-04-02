<template>
  <button @click="toggleDarkMode" class="bg-slate-100 dark:bg-slate-800">
    {{ `${isDarkMode ? "Light" : "Dark"}` }}
  </button>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const isDarkMode = ref(false);

    const toggleDarkMode = () => {
      isDarkMode.value = !isDarkMode.value;
      document.body.classList.toggle("dark", isDarkMode.value);
    };

    return { isDarkMode, toggleDarkMode };
  },
};
</script>
