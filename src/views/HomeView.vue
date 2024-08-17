<script setup>

import Filters from '@/components/Filters.vue';
import Card from '@/components/Card.vue';
import { RouterLink } from 'vue-router';

import { watch, onMounted, ref  } from 'vue';

const activeFilters = ref({}),
      countries = ref([]),
      allFetchedCountries = ref([]);
  
const setFilters = (filters) => {
  activeFilters.value = { ...activeFilters.value, ...filters };
};

const fetchCountries = async () => {
  let response;
  let data = [];
  try {
    if (activeFilters.value.regionFilter) {
      response = await fetch(`https://restcountries.com/v3.1/region/${activeFilters.value.regionFilter}`);
    } else {
      response = await fetch("https://restcountries.com/v3.1/all");
    }
    if (!response.ok) throw new Error(`Failed to get data. status: ${response.status}`);

    data = await response.json();

    allFetchedCountries.value = data;

    filterCountries();
  } catch (err) {
    console.error("Error fetching data: ", err);
  }
};

const filterCountries = () => {
  let filteredCountries = allFetchedCountries.value;
  if (activeFilters.value.searchTerm) {
    const searchTermLower = activeFilters.value.searchTerm.toLowerCase();
    filteredCountries = filteredCountries.filter(country => country.name.common.toLowerCase().includes(searchTermLower));
  }
  countries.value = filteredCountries;
};

watch(activeFilters, (newFilters, oldFilters) => {
  if (newFilters.regionFilter !== oldFilters.regionFilter) {
    fetchCountries();
  } else if (newFilters.searchTerm !== oldFilters.searchTerm) {
    filterCountries();
  }
}, { deep: true });

onMounted(fetchCountries);

</script>

<template>

<div class="page__content">
      <Filters @set-filters="setFilters"/>
    </div>

    <ul class="country-list">
      <li v-for="(country, index) in countries" :key="index">
        <RouterLink :to="{ name: 'country', params: { id: country.name.common }}">
          <Card :country="country" />
        </RouterLink>
      </li>
    </ul>

    <div v-if="countries.length === 0" class="no-results">
      <p> No results </p>
    </div>
</template>


<style lang="scss" scoped>
  .page__content {
    padding: 1rem;
  }

  .country-list {
    padding: 1rem;

    li {
      list-style: none;
      margin-block-end: 1rem;
    }
  }

  @media only screen and (min-width: 550px) {
    .country-list {
      columns: 200px 2;
      gap: 1rem;

      li {
        break-inside: avoid;
      }
  }
  }

  @media only screen and (min-width: 800px) {
    .country-list {
      columns: 200px 3;
    }
  }

  @media only screen and (min-width: 1200px) {
    .country-list {
      columns: 200px 4;
    }
  }

  @media only screen and (min-width: 1600px) {
    .country-list {
      columns: 200px 5;
    }
  }
</style>