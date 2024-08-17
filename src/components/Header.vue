<script setup>
    import { ref, defineEmits} from 'vue';
    import { RouterLink } from 'vue-router';

    const darkMode = ref(false),
          emit = defineEmits(['toggle-theme'])

    function updateTheme() {
        darkMode.value = !darkMode.value;
        emit('toggle-theme', darkMode.value)
    }

</script>

<template>

    <header class="header">
        <RouterLink :to="{ name: 'home' }">
            <h1 class="header__title">Where in the world?</h1>
        </RouterLink>

        <button class="header__toggle" @click="updateTheme"> <span class="material-symbols-outlined">{{ darkMode ? "lightbulb" : "dark_mode"}} </span></button>
    </header>

</template>

<style lang="scss"> 

  .header {
    align-items: center;
    background-color: var(--base-color);
    color: var(--text-color);
    display: flex;
    justify-content: space-between;
    padding: 1rem;
    box-shadow: 0 5px 5px 0 rgba(0,0,0,0.25);

    &__title {
        font-size: 1.25rem;
        font-weight: 800;
    }

    &__toggle {
        align-items: center;
        border: 0;
        background-color: transparent;
        display: flex;
        gap: .5rem;
        font-size: 1rem;
        font-weight: 600;
        padding: 1rem;
        color: var(--text-color)
    }
  }
</style>