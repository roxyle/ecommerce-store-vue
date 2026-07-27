<script setup lang="ts">
import { getProduct } from '@/api/fakeStoreApi';
import type { Product } from '@/types/product';
import { formatPrice } from '@/utils/formatPrice';
import { ref, watch } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const product = ref<Product>()
const loading = ref(true)
const error = ref<string | null>(null)


watch(
  () => route.params.id,
  () => {
    loading.value = true;
    error.value = null;

    getProduct(Number(route.params.id))
      .then((data) => {
        product.value = data;
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
  <p v-if="loading" aria-busy="true" aria-live="polite">Caricamento prodotto in corso...</p>

  <p v-else-if="error">{{ error }}</p>
  <p v-else-if="!product">Nessun prodotto trovato per questo id.</p>

  <div v-else :class="$style.detail">
    <img :src="product.image" :alt="product.title" :class="$style.image" />
    <div :class="$style.info">
      <h2 :class="$style.title">{{ product.title }}</h2>
      <p :class="$style.price">{{ formatPrice(product.price) }}</p>
      <p :class="$style.description">{{ product.description }}</p>
    </div>
  </div>
</template>


<style module>
.detail {
    display: flex;
    flex-direction: column;
    gap: var(--spacing-md);
    padding: var(--spacing-md);
    max-width: 1024px;
    margin: 0 auto;
}

.image {
    width: 100%;
    max-height: 400px;
    object-fit: contain;
    background-color: var(--bg-primary);
}

.info {
    display: flex;
    flex-direction: column;
    gap: var(--spacing-sm);
}

.title {
    margin: 0;
    font-size: var(--font-size-lg);
    color: var(--text-primary);
}

.price {
    margin: 0;
    font-size: var(--font-size-xl);
    font-weight: 800;
    color: var(--text-primary);
}

.description {
    margin: 0;
    color: var(--text-secondary);
    line-height: 1.6;
}

/* Da tablet in su: immagine a sinistra, testo a destra */
@media (min-width: 768px) {
    .detail {
        flex-direction: row;
        align-items: flex-start;
        gap: var(--spacing-xl);
    }

    .image {
        width: 50%;
    }
}
</style>
