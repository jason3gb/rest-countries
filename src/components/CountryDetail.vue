<template>
  <div class="country-detail">
    <button @click="onExit" class="back-button">
      <span class="back-button-icon"></span>
      <span>Back</span>
    </button>
    <div class="main-section">
      <div class="flag">
        <img :src="flagSrc" alt="Flag of {{ country.flags[0] }}"/>
      </div>
      <div class="info">
        <h2>{{ country.name.common }}</h2>
        <div class="country-details">
          <div class="detail-basic">
            <span><strong>Native Name:</strong> {{ nativeName }}</span>
            <span><strong>Population:</strong> {{ country.population }}</span>
            <span><strong>Region:</strong> {{ country.region }}</span>
            <span v-if="country.subregion"><strong>Sub Region:</strong> {{ country.subregion }}</span>
            <span><strong>Capital:</strong> {{ capital }}</span>
          </div>
          <div class="detail-extra">
            <span><strong>Top Level Domain:</strong> {{ topLevelDomain }}</span>
            <span><strong>Currencies:</strong> {{ currencies }}</span>
            <span><strong>Languages:</strong> {{ languages }}</span>
          </div>
        </div>
        <div class="border-countries">
          <div class="attribute-label">Border Countries:</div>
          <div v-for="bc in borderCountries" class="border-country">{{ bc }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>

import {onMounted, computed, onUnmounted} from "vue";

const props = defineProps(
    ['country', 'borders']
)

const emits = defineEmits(
    ['on-exit']
)

// computed properties
const flagSrc = computed(() => {
  if (!props.country.flags || !props.country.flags['png']) {
    return '#';
  }

  return props.country.flags['png'];
})


const nativeName = computed(() => {
  const nativeNameObject = props.country.name.nativeName;
  if (nativeNameObject === undefined || Object.keys(nativeNameObject).length === 0) {
    return '';
  }

  const firstLanguageKey = Object.keys(nativeNameObject)[0];

  return nativeNameObject[firstLanguageKey].common;
})

const topLevelDomain = computed(() => {
  const tld = props.country.tld;
  return tld && tld.length > 0 ? tld[0] : '';
})

const capital = computed(() => {
  if (!props.country.capital || props.country.capital.length < 1) {
    return 'N/A';
  }

  return props.country.capital[0];
})

const currencies = computed(() => {
  const currencies = props.country.currencies;
  const currencyKeys = Object.keys(currencies);
  return currencyKeys.length > 0 ? currencies[currencyKeys[0]].name : 'N/A';
})

const languages = computed(() => {
  const languages = props.country.languages;
  const languageValues = Object.values(languages);
  return languageValues.length > 0 ? languageValues.join(', ') : 'N/A';
})

const borderCountries = computed(() => {
  const borders = props.borders;
  return borders.length > 0 ? borders : ['None'];
})

// handlers
const onExit = () => {
  emits('on-exit');
}

</script>

<style lang="scss" scoped>

@import '@/assets/styles/mixin';

.country-detail {
  background-color: var(--color-background);

  position: fixed;
  top: 0;
  left: 0;
  z-index: 1000;
  overflow-y: auto;

  height: calc(100vh - var(--navbar-height));
  width: 100%;

  padding: 80px;
  box-sizing: border-box;

  font-weight: var(--font-weight-light);

  @include mobile {
    padding: 8%;
  }

  strong {
    font-weight: var(--font-weight-semi-bold);
  }

  .back-button {
    display: flex;
    align-items: center;
    gap: 10px;

    padding: 10px 30px;
    border-radius: 6px;

    border: none;

    background-color: var(--color-button-color);
    color: var(--color-button-text);

    box-shadow: 0 0 7px rgba(0, 0, 0, 10%);

    [data-theme="dark"] & {
      box-shadow: none;
    }

    &:hover {
      cursor: pointer;
    }

    span {
      display: inline-block;
    }

    .back-button-icon {
      width: 20px;
      height: 20px;
      background: url('@/assets/images/back-arrow.svg') no-repeat center center / contain;

      [data-theme="dark"] & {
        filter: invert(100%);
      }
    }
  }

  .main-section {
    margin-top: 80px;

    display: flex;
    align-items: center;
    gap: 8%;

    @include mobile {
      margin-top: 60px;
      flex-direction: column;
      align-items: flex-start;
      gap: 20px;
    }

    .flag {
      flex-grow: 1;
      flex-basis: 550px;
      width: 100%;

      @include mobile {
        flex-grow: 0;
        flex-basis: 100%;
      }

      img {
        width: 100%;
        object-fit: fill;
        border-radius: 5px;
      }
    }

    .info {
      flex-grow: 1;
      flex-basis: 600px;

      @include mobile {
        flex-grow: 0;
        flex-basis: 100%;
      }

      h2 {
        font-weight: var(--font-weight-bold);
      }

      .country-details {
        margin-top: 5%;

        display: flex;
        gap: 5%;

        .detail-basic {
          flex-grow: 1;
          display: flex;
          flex-direction: column;
          gap: 5px;
        }

        .detail-extra {
          flex-grow: 1;
          display: flex;
          flex-direction: column;
          gap: 5px;
        }

        @include mobile {
          margin-top: 20px;
          flex-direction: column;
          gap: 30px;
        }
      }

      .border-countries {
        margin-top: 10%;
        display: flex;
        flex-wrap: wrap;
        align-items: center;

        gap: 10px;

        @include mobile {
          margin-top: 40px;
        }

        .attribute-label {
          margin-right: 6px;
          font-weight: var(--font-weight-semi-bold);
          font-size: 16px;

          @include mobile {
            width: 100%;
          }
        }

        .border-country {
          padding: 5px 27px;
          box-shadow: 0 0 4px rgba(0, 0, 0, 0.15); /* Add shadow for depth */

          [data-theme="dark"] & {
            background-color: var(--dark-blue);
          }
        }
      }
    }
  }

}


</style>