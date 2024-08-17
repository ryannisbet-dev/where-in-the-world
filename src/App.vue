<script setup>
import { ref  } from 'vue';

import Header from '@/components/Header.vue';
import { RouterView } from 'vue-router';

const darkMode = ref(false);

const toggleTheme = () => {
  darkMode.value = !darkMode.value;
};
</script>

<template>
  <div :class="`page-container ${darkMode ? 'dark-mode' : ''}`">
    <Header @toggle-theme="toggleTheme" />
    <RouterView/>
    
  </div>
</template>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  font-family: var(--font);
}

a {
  color: var(--text-color);
  text-decoration: none;
}

.page-container {
  background-color: var(--background-color);
  color: var(--text-color);
  min-height: 100vh;
}

.u-input,
.btn {
  background-color: var(--base-color);
  border: 0;
  border-radius: 5px;
  color: var(--text-color);
  padding: .5rem;
}
</style>
