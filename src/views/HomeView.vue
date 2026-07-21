<script setup lang="ts">
import { ref, watch } from 'vue';
import { useRoute } from 'vue-router';
import { getAllProducts, getProductsByCategory } from '../api/fakeStoreApi';
import type { Product } from '../types/product';
import ProductCard from '../components/ProductCard/ProductCard.vue';
import ProductCardSkeleton from '../components/Skeleton/ProductCardSkeleton.vue';

const route = useRoute();

const products = ref<Product[]>([]);
const loading = ref(true);
const error = ref<string | null>(null);

watch(
  () => route.query.category,
  (category) => {
    loading.value = true;
    error.value = null;

    const request = category
      ? getProductsByCategory(category as string)
      : getAllProducts();

    request
      .then((data) => {
        products.value = data;
        loading.value = false;
      })
      .catch((err: Error) => {
        error.value = err.message;
        loading.value = false;
      });
  },
  { immediate: true }
);
</script>

<template>
  <main>
    <p v-if="loading" aria-busy="true" aria-live="polite">Caricamento prodotti in corso...</p>

    <div v-if="loading" :class="$style.grid">
      <ProductCardSkeleton v-for="n in 8" :key="n" />
    </div>
    <p v-else-if="error">{{ error }}</p>
    <p v-else-if="products.length === 0">Nessun prodotto trovato per questa categoria.</p>
    <div v-else :class="$style.grid">

      <ProductCard v-for="product in products" :key="product.id" :product="product" />
    </div>

  </main>
</template>

<style module>
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: var(--spacing-lg);
  padding: var(--spacing-lg);
}
</style>
