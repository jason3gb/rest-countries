<template>
  <div ref="dropdown" class="dropdown">
    <button class="dropdown-btn" @click="toggleDropdown">
      <span>{{ selectedRegion || 'Filter by Region' }}</span>
      <span v-if="selectedRegion" class="clear-icon" @click.stop="clearSelection">&times;</span>
      <span class="arrow"></span>
    </button>
    <div class="dropdown-content" v-show="isOpen">
      <a href="#" v-for="region in regions" :key="region" @click="selectRegion(region, $event)">{{ region }}</a>
    </div>
  </div>
</template>

<script setup>

import {onMounted, onUnmounted, ref} from 'vue';

const emit = defineEmits(['on-filter-change']);

const regions = ['Africa', 'Americas', 'Asia', 'Europe', 'Oceania'];
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

const clearSelection = () => {
  selectedRegion.value = ''; // Clear the selected region
  emit('on-filter-change', ''); // Emit the change with an empty value
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

  width: 200px;
}

.dropdown-btn {
  padding: 18px 24px;

  background-color: var(--color-tool-background);
  color: var(--color-tool-text);

  border: none;
  cursor: pointer;
  border-radius: 5px;
  width: 100%; /* Adjust as needed */
  text-align: left;

  display: flex;
  align-items: center;
  justify-content: space-between;

  .arrow {
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

.clear-icon {
  cursor: pointer;
  margin-right: 10px;
  font-size: 14px; /* Adjust size as needed */

  width: 16px; /* Ensure this is the same as the height */
  height: 16px; /* Ensure this is the same as the width */

  line-height: 16px; /* Align the '×' character vertically */
  text-align: center;

  padding: 1px; /* Give some space around the 'x' for a better click area */
  border-radius: 50%; /* Optional: round shape for the hover effect */

  &:hover {
    background-color: #e0e0e0; /* A light grey background on hover */
    color: #333; /* Darker text color to stand out */
  }
}

</style>
