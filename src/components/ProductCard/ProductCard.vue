<template>
  <RouterLink :class="$style.card" :to="`/product/${product.id}`">
    <img :class="$style.image" :src="product.image" :alt="product.title" />
    <div :class="$style.info">
      <h3>{{ product.title }}</h3>
      <span>{{ formatPrice(product.price) }}</span>
    </div>
  </RouterLink>
</template>




<script setup lang="ts">
import type { Product } from '../../types/product';
import { formatPrice } from '../../utils/formatPrice';


defineProps<{ product: Product }>();


</script>




<style module>
  .card {
    display: flex;
    flex-direction: column;
    background-color: var(--bg-secondary);
    border-radius: var(--border-radius);
    overflow: hidden;
    text-decoration: none;
    color: inherit;
    padding-top: var(--spacing-sm);
}

.image {
    width: 100%;
    height: 200px;
    object-fit: contain;
    background-color: var(--bg-secondary);
}

.info {
    flex: 1;
    padding: var(--spacing-md);
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    gap: var(--spacing-sm);
}

.info h3,
.info span {
    margin: 0;
}
</style>
