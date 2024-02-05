<template>
  <div ref="dropdown" class="dropdown">
    <button class="dropdown-btn" @click="toggleDropdown">
      <span>{{ selectedRegion || 'Filter by Region' }}</span>
      <span class="arrow"></span>
    </button>
    <div class="dropdown-content" v-show="isOpen">
      <a href="#" v-for="region in regions" :key="region" @click="selectRegion(region, $event)">{{ region }}</a>
    </div>
  </div>
</template>

<script setup>

import {onMounted, onUnmounted, ref} from 'vue';

const emit = defineEmits(['on-filter-change'])

const regions = ['Africa', 'America', 'Asia', 'Europe', 'Oceania'];
const isOpen = ref(false);
const selectedRegion = ref('');

const toggleDropdown = () => {
  isOpen.value = !isOpen.value;
};

const selectRegion = (region, event) => {
  event.preventDefault();

  selectedRegion.value = region;
  isOpen.value = false;

  emit('on-filter-change', region);
};

// Close the dropdown if clicked outside
const closeDropdown = (event) => {
  if (!event.target.closest('.dropdown-btn') && !event.target.closest('.dropdown-content')) {
    isOpen.value = false;
  }
};

onMounted(() => {
  window.addEventListener('click', closeDropdown);
});

onUnmounted(() => {
  window.removeEventListener('click', closeDropdown);
});

</script>

<style lang="scss" scoped>

.dropdown {
  position: relative;
  display: inline-block;

  box-shadow: 0 4px 9px rgba(0, 0, 0, 5%);
}

.dropdown-btn {
  padding: 18px 24px;

  background-color: var(--color-tool-background);
  color: var(--color-tool-text);

  border: none;
  cursor: pointer;
  border-radius: 5px;
  width: 200px; /* Adjust as needed */
  text-align: left;

  display: flex;
  align-items: center;

  .arrow {
    margin-left: auto;
    background: url("@/assets/images/expand-more.svg") no-repeat center center / contain;
    width: 10px;
    height: 6px;
  }
}

.dropdown-content {
  position: absolute;
  background-color: var(--color-tool-background);
  min-width: 200px; /* Adjust as needed */
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
  z-index: 1;
  top: 100%;
  left: 0;
}

.dropdown-content a {
  color: var(--color-tool-text);
  padding: 12px 16px;
  text-decoration: none;
  display: block;
}

.dropdown-content a:hover {
  background-color: var(--filter-box-highlight);
}

</style>
