<template>
  <div class="country-card">
    <div class="flag">
      <img :src="country.flags['png']" alt="Flag of {{ country.flags[0] }}"/>
    </div>

    <div class="country-info">
      <h3>{{ country.name.official }}</h3>
      <span><strong>Population: </strong>{{ country.population }}</span>
      <span><strong>Region: </strong>{{ country.region }}</span>
      <span><strong>Capital: </strong>{{ country.capital[0] }}</span>
    </div>
  </div>
</template>

<script setup>

defineProps(
    ['country']
)


</script>

<style lang="scss" scoped>

.country-card {
  min-width: 264px;
  min-height: 336px;

  border-top-left-radius: 5px;
  border-top-right-radius: 5px;

  background-color: var(--color-country-card-background);

  display: flex;
  flex-direction: column;

  .flag {
    width: 100%;
    height: 160px;

    img {
      width: 100%;
      height: 100%;
      object-fit: cover;

      border-top-left-radius: 5px;
      border-top-right-radius: 5px;
    }
  }

  .country-info {
    padding: 24px;

    span {
      display: block;
      margin-top: 8px;
    }
  }

  h3 {
    margin: 0;
  }
}

</style>