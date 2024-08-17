<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const countryData = ref(null);
const countryCodeToNameMap = ref({});

onMounted(async () => {
    await fetchCountryById(route.params.id); 
});


const fetchCountryById = async (id) => {
  try {
    const response = await fetch(`https://restcountries.com/v3.1/name/${id}`);
    const data = await response.json();

    countryData.value = data[0];

  } catch (err) {
    console.error("Error fetching data: ", err);
  }
};

</script>

<template>
  <div v-if="countryData" class="country">
    <button class="btn country__btn" @click="$router.go(-1)">Back</button>

    <div class="country__container">
        <img class="country__flag" :src="`${countryData.flags.png}`"/>

        <div class="country__about">
            <h1 class="country__title"> {{ countryData.name.common }}</h1>

            <div class="country__data country__data--nativeName" v-if="countryData.name && Object.keys(countryData.name.nativeName).length > 0">
                <h3> Native Names: </h3>
                <p v-for="lang in Object.keys(countryData.languages)" :key="lang"> <h4>{{ `${lang.toUpperCase()}:` }}</h4> {{ countryData.name.nativeName[lang].common }}</p>
            </div>

            <div class="country__data">
                <h3> Population: </h3>
                <p> {{ countryData.population }}</p>
            </div>

            <div class="country__data">
                <h3> Region: </h3>
                <p> {{ countryData.region}}</p>
            </div>

            <div class="country__data" v-if="countryData.subregion">
                <h3> Sub Region: </h3>
                <p> {{ countryData.subregion }}</p>
            </div>

            <div class="country__data">
                <h3> Capital: </h3>
                <p>{{ Array.isArray(countryData.capital) ? countryData.capital.join(', ') : countryData.capital || 'N/A' }}</p>
            </div>

            <div class="country__data" v-if="countryData.currencies">
                <h3> Currencies: </h3>
                <p v-for="currency in countryData.currencies" :key="currency">{{ currency.name }}</p>
            </div>

            <div class="country__data" v-if="countryData.languages">
                <h3> Languages: </h3>
                <p>{{ Object.values(countryData.languages).join(', ') }}</p>
            </div>

        </div>
    </div>

  </div>

  <div v-else>
    <h1>Country doesn't exist</h1>
  </div>
</template>

<style lang="scss" scoped>
    .country {
        display: flex;
        flex-direction: column;
        gap: 1rem;
        padding: 1rem;

        &__btn {
            max-width: 150px;
        }

        &__title {
            font-size: 1.75rem;
            font-weight: 800;
            margin-block: 1rem;
        }

        &__about {
            display: flex;
            flex-direction: column;
            flex-wrap: wrap;
            gap: 1rem;
        }

        &__data {
            align-items: center;
            display: flex;
            gap: 1rem;

            &--nativeName {
                align-items: flex-start;
                flex-direction: column;
            }
        }

        &__flag {
            border-radius: 10px;
            width: 100%;
        }
    }

    @media (min-width: 1000px) { 

        .country {
            &__container {
                align-items: center;
                display: flex;
                justify-content: space-around;
            }

            &__flag {
                width: 400px;
            }
        }
    }

@media (min-width: 1300px) { 

    .country__flag {
        width: 600px;
    }
}
</style>