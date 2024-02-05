<template>
  <div class="search-container">
    <input @input="debounceInput" type="text" placeholder="Search for a country..." class="search-input"/>
  </div>
</template>

<script setup>

import _ from 'lodash';
import { ref } from 'vue';

const emit = defineEmits(['on-search']);

const searchText = ref('');

const onInputChange = (event) => {
  searchText.value = event.target.value;
  emit('on-search', searchText.value);
}

const debounceInput = _.debounce(onInputChange, 300);

</script>

<style scoped>
.search-container {
  max-width: 680px; /* Adjust as needed */
  width: 30%;
  padding: 18px;
  box-shadow: 0 4px 9px rgba(0, 0, 0, 5%);
  border-radius: 5px;

  background-color: var(--color-tool-background);
  color: var(--color-tool-text);

  box-sizing: border-box;

  position:relative;
}

.search-input {
  border: none;
  outline: none;
  width: 100%;
  margin-left: 16px;
  font-size: 16px; /* Adjust as needed */

  box-sizing: border-box;

  background-color: var(--color-tool-background);
  color: var(--color-tool-text);

  padding: 0 18px 0 32px;
}

.search-input::placeholder {
  color: var(--color-tool-placeholder);
}

/* You might want to add ::webkit-search-cancel-button to remove clear icon in some browsers */
.search-input::-webkit-search-cancel-button {
  -webkit-appearance: none;
  height: 1rem;
  width: 1rem;
  display: none;
}

/* Add a pseudo-element for the search icon */
.search-container::before {
  content: '';
  position: absolute;
  left: 32px; /* Adjust as needed */
  top: 50%;
  transform: translateY(-50%);
  background: url('@/assets/images/search.svg') no-repeat center center;
  background-size: contain;
  width: 18px; /* Adjust based on the size of your icon */
  height: 18px; /* Adjust based on the size of your icon */
}

</style>