<script setup>
    const props = defineProps({
        country: Object,
    });
</script>

<template>
    <div class="card">
        <img :src="`${props.country.flags.png}`" class="card__img"/>

        <div class="card__text-container">
            <h1 class="card__title"> {{ props.country.name.common }}</h1>
            <p class="card__text"> <span class="card__subtitle">Population:</span> {{ props.country.population }}</p>
            <p class="card__text"><span class="card__subtitle">Region:</span> {{ props.country.region}}</p>
            <p class="card__text"><span class="card__subtitle">Capital:</span> {{ Array.isArray(props.country.capital) ? props.country.capital.join(', ') : props.country.capital || 'N/A' }}</p>
        </div>
    </div>
</template>

<style lang="scss" scoped>

    .card {
        background-color: var(--base-color);
        border-radius: 5px;
        width: 100%;
        height: 100%;

        &__img { 
            width: 100%;
            border-start-start-radius: 5px;
            border-start-end-radius: 5px;
        }
        
        &__text-container {
            display: flex;
            flex-direction: column;
            gap: .5rem;
            padding: 1rem;
        }

        &__subtitle {
            font-weight: 700;
            margin-inline-end: .25rem;
        }
    }
</style>
