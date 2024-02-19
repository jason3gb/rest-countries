<template>
  <div @click="onCardClick" class="country-card">
    <div class="flag">
      <img :src="country.flags['png']" alt="Flag of {{ country.flags[0] }}"/>
    </div>

    <div class="country-info">
      <h3>{{ country.name }}</h3>
      <span><strong>Population: </strong>{{ country.population }}</span>
      <span><strong>Region: </strong>{{ country.region }}</span>
      <span><strong>Capital: </strong>{{ country.capital }}</span>
    </div>
  </div>
</template>

<script setup>

const props = defineProps(
    ['country']
)

const emit = defineEmits(['on-show-country-detail']);

const onCardClick = () => {
  emit('on-show-country-detail', props.country);
}

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

  font-weight: var(--font-weight-light);

  strong {
    font-weight: var(--font-weight-semi-bold);
  }

  &:hover {
    cursor: pointer;

    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Add shadow for depth */
    transform: translateY(-5px); /* Slightly lift the card */
  }

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