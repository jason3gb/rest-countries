<template>
  <button @click="toggleTheme" class="theme-toggle">
    <span class="moon-icon" :style="{
      background: `url(${themeIconName}) no-repeat center center / contain`
    }"></span>
    <span>Dark Mode</span>
  </button>
</template>

<script setup>
import {computed, ref} from 'vue';

import IconDarkModeOn from '@/assets/images/dark-mode-on.svg';
import IconDarkModeOff from '@/assets/images/dark-mode-off.svg';

const theme = ref('light');

const toggleTheme = () => {
  theme.value = theme.value === 'light' ? 'dark' : 'light';
  document.documentElement.setAttribute('data-theme', theme.value);
};

const themeIconName = computed(() => {
  return theme.value === 'light' ? IconDarkModeOff : IconDarkModeOn;
});

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

  .moon-icon {
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