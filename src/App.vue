<script setup lang="ts">
import { ref, watch } from 'vue';
import AppHeader from './components/Header/AppHeader.vue';

const theme = ref<'light' | 'dark'>('light');

function toggleTheme() {
  theme.value = theme.value === 'light' ? 'dark' : 'light';
}

watch(
  theme,
  (newTheme) => {
    document.documentElement.setAttribute('data-theme', newTheme);
  },
  { immediate: true }
);
</script>

<template>
  <AppHeader :theme="theme" :onToggleTheme="toggleTheme" />
  <main>
    <RouterView v-slot="{ Component }">
      <component :is="Component" :key="$route.fullPath" />
    </RouterView>
  </main>
</template>
