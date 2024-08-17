<script setup>
import { ref, defineEmits } from 'vue';

const searchTerm = ref("");
const regionFilter = ref("");
const emit = defineEmits(['set-filters']);

function updateSearchTerm() {
  emit('set-filters', { searchTerm: searchTerm.value });
}

function updateRegionFilter() {
  emit('set-filters', { regionFilter: regionFilter.value });
}

function clearFilters() {
  searchTerm.value = "";
  regionFilter.value = "";

  updateSearchTerm();
  updateRegionFilter();
}
</script>

<template>
  <div class="filters">

    <div class="filters__search">
      <input type="text" v-model="searchTerm" @keyup="updateSearchTerm" class="search__input u-input">
      <span class="material-symbols-outlined">search</span>
      </input>
    </div>

    <div class="filters__region">

      <select v-model="regionFilter" @change="updateRegionFilter" class="filters__dropdown u-input">
        <option value="" disabled selected>Filter by Region</option>
        <option value="africa">Africa</option>
        <option value="america">America</option>
        <option value="asia">Asia</option>
        <option value="europe">Europe</option>
        <option value="oceania">Oceania</option>
      </select>

      <button v-if="searchTerm || regionFilter" @click="clearFilters" class="material-symbols-outlined btn">filter_alt_off</button>

    </div>
  
  </div>
</template>

<style lang="scss" scoped>
    .filters {
        display: flex;
        flex-direction: column;
        gap: 2rem;
        justify-content: space-between;

        &__search {
          align-items: center;
          background-color: var(--base-color);
          border-radius: 5px;
          display: flex;
          flex-grow: 1;
          padding: .5rem;
        }

        &__dropdown {
          padding: 1rem;
          width: 70%;
        }

        &__region {
          display: flex;
          justify-content: space-between;
          
          .btn {
            padding-inline: 1rem;
          }
        }
    }

    .search {
      &__input {
        flex-grow: 1;
        outline: 0;
      }

    }

    @media only screen and (min-width: 800px) {
      .filters__dropdown {
        width: 40%;
      }
    }
</style>
