<template>
  <div class="theme-toggle">
    <button @click="toggleTheme">Toggle Theme</button>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const theme = ref('light');

const toggleTheme = () => {
  theme.value = theme.value === 'light' ? 'dark' : 'light';
  document.documentElement.setAttribute('data-theme', theme.value);
};

</script>

<style scoped>

.theme-toggle {
  display: flex;
  align-items: center;
  justify-content: center;
}

</style>