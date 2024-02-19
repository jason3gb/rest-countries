<template>
  <div class="main-view">
    <div class="tools">
      <SearchBox @on-search="onSearchInputChange"/>
      <FilterBox @on-filter-change="onFilterChange"/>
    </div>
    <div class="country-list">
      <CountryCard v-for="country in countries"
                   :country="country"
                   :key="country.name"
                   @on-show-country-detail="onShowCountryDetail"
      />
    </div>
    <div v-if="showDetailOverlay">
      <CountryDetail
          :country="currentCountry"
          :borders="currentCountryBorders"
          @on-exit="onCountryDetailExit"
      />
    </div>
    <loading
        :active="isDataLoading"
        :can-cancel="false"
        :is-full-page="true"
    />
  </div>
</template>

<script setup>
import Loading from 'vue-loading-overlay';
import 'vue-loading-overlay/dist/css/index.css';

import {ref, onMounted, computed} from "vue";

import SearchBox from "@/components/SearchBox.vue";
import FilterBox from "@/components/FilterBox.vue";
import CountryCard from "@/components/CountryCard.vue";

import CountryDetail from "@/components/CountryDetail.vue";

const defaultCountry = {
  name: {},
  population: 0,
  region: '',
  capital: [],
  flags: {},
  code: '',
  nativeName: {},
  tld: [],
  currencies: [],
  languages: {},
}

const allCountries = ref([]);
const countries = ref([]);

const showDetailOverlay = ref(false);

const currentCountry = ref({...defaultCountry});

const currentCountryBorders = ref([]);

const isDataLoading = ref(false);

const onSearchInputChange = (searchText) => {
  if (searchText === '') {
    countries.value = allCountries.value;
    return;
  }

  countries.value = allCountries.value.filter(country => country.name.toLowerCase().includes(searchText.toLowerCase()));
}

const onFilterChange = (region) => {
  if (region === '') {
    countries.value = allCountries.value;
    return;
  }

  countries.value = allCountries.value.filter(country => country.region.toLowerCase() === region.toLowerCase());
}

const code2Name = computed(() => {
  const map = {};
  allCountries.value.forEach(country => {
    map[country.code] = country.name;
  });

  return map;
})

const onShowCountryDetail = (country) => {
  isDataLoading.value = true;

  const countryCommonName = country.name;
  fetch(`https://restcountries.com/v3.1/name/${countryCommonName}?fullText=true`)
      .then(response => response.json())
      .then(data => {
        if (!data || data.length === 0) {
          throw new Error('No country data found');
        }

        currentCountry.value = data[0];

        if (!data[0].borders || data[0].borders.length === 0) {
          currentCountryBorders.value = [];
        } else {
          currentCountryBorders.value = data[0].borders.map(border => code2Name.value[border]);
        }

        showDetailOverlay.value = true;
      })
      .catch(error => {
        console.error('Error fetching country data:', error);
      })
      .finally(() => {
        isDataLoading.value = false;
      })
}

const onCountryDetailExit = () => {
  currentCountry.value = {...defaultCountry};
  currentCountryBorders.value = [];

  showDetailOverlay.value = false;
}

onMounted(() => {
  isDataLoading.value = true;
  fetch(`https://restcountries.com/v3.1/all`)
      .then(response => response.json())
      .then(data => {
        if (!data || data.length === 0) {
          throw new Error('country data load error');
        }

        allCountries.value = data.map((country) => {
          let capital = country.capital && country.capital.length > 0 ? country.capital[0] : 'N/A';

          return {
            name: country.name.common,
            population: country.population,
            region: country.region,
            capital: capital,
            flags: country.flags,
            code: country.cca3,
          }
        })

        countries.value = allCountries.value;
      })
      .catch(error => {
        console.error('Error fetching country data:', error);
      })
      .finally(() => {
        isDataLoading.value = false;
      })
})

</script>

<style lang="scss" scoped>

.main-view {
  padding: 3% 5%;

  box-sizing: border-box;
  position: relative;
}

.tools {
  margin-top: var(--navbar-height);
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

:deep(.country-detail) {
  margin-top: var(--navbar-height);
}


</style>


