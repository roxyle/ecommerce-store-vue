<script setup lang="ts">
  import SkeletonBox from './SkeletonBox.vue';
</script>

<template>
  <div :class="$style.card">
    <SkeletonBox width="100%" height="180px" borderRadius="8px" />
    <SkeletonBox width="80%" height="20px" borderRadius="4px" />
    <SkeletonBox width="40%" height="24px" borderRadius="4px" />
  </div>
</template>

<style module>
.card {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-sm);
  padding: var(--spacing-md);
  border: 1px solid var(--color-border);
  border-radius: var(--border-radius);
  width: 100%;
  background-color: var(--bg-secondary);
}
</style>
