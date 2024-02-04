<template>
  <div class="main-view">
    <div class="tools">
      <SearchBox />
      <FilterBox />
    </div>
    <div class="country-list">
      <CountryCard />
    </div>
  </div>
</template>

<script setup>
import SearchBox from "@/components/SearchBox.vue";
import FilterBox from "@/components/FilterBox.vue";
import CountryCard from "@/components/CountryCard.vue";
</script>

<style scoped>

.main-view {
  padding: 5%;
}

.tools {
  display: flex;
  align-items: center;
}

.country-list {
  margin-top: 5%;
}


:deep(.filter-box) {
  margin-left: auto;
}


</style>


