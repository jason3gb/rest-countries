<template>
  <div class="main-view">
    <div class="tools">
      <SearchBox @on-search="onSearchInputChange"/>
      <FilterBox @on-filter-change="onFilterChange"/>
    </div>
    <div class="country-list">
      <CountryCard v-for="country in countries" :country="country" :key="country.name"/>
    </div>
  </div>
</template>

<script setup>
import {ref, onMounted} from "vue";

import SearchBox from "@/components/SearchBox.vue";
import FilterBox from "@/components/FilterBox.vue";
import CountryCard from "@/components/CountryCard.vue";

import countryData from '@/data/resp.json';

const countries = ref([]);

const onSearchInputChange = (searchText) => {
  console.log('Search input changed, new search text:', searchText);
}

const onFilterChange = (region) => {
  console.log('Filter changed, new region:', region);
}

onMounted(() => {
  countries.value = countryData.map((country) => {
    return {
      name: country.name,
      population: country.population,
      region: country.region,
      capital: country.capital,
      flags: country.flags,
    }
  })
})

</script>

<style scoped>

.main-view {
  padding: 3% 5%;

  box-sizing: border-box;
}

.tools {
  display: flex;
  align-items: center;
}

.country-list {
  margin-top: 5%;

  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 75px;
}


:deep(.dropdown) {
  margin-left: auto;
}


</style>


