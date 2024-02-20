<template>
  <button @click="toggleTheme" class="theme-toggle">
    <span v-if="isLight" class="icon-light"></span>
    <span v-else class="icon-dark"></span>
    <span>Dark Mode</span>
  </button>
</template>

<script setup>
import {ref, computed} from 'vue';

const theme = ref('light');

const toggleTheme = () => {
  theme.value = theme.value === 'light' ? 'dark' : 'light';
  document.documentElement.setAttribute('data-theme', theme.value);
};

const isLight = computed(() => theme.value === 'light');

</script>

<style lang="scss" scoped>

.theme-toggle {
  background: none;
  border: none;
  outline: none;

  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;

  transition: transform 0.1s ease;

  &:active {
    transform: scale(0.95);
  }

  .icon-light {
    background: url('@/assets/images/light-moon.svg') no-repeat center center / contain;
    margin-right: 8px; /* Space between the icon and text */
    display: inline-block;

    height: 20px;
    width: 20px;
  }

  .icon-dark {
    background: url('@/assets/images/dark-moon.svg') no-repeat center center / contain;
    margin-right: 8px; /* Space between the icon and text */
    display: inline-block;

    height: 20px;
    width: 20px;
  }

  span {
    color: var(--color-text);
  }
}

.moon-icon {
  margin-right: 8px; /* Space between the icon and text */
}

</style>