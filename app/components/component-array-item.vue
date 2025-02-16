<script setup lang="ts" generic="T extends Item">
import type { Item } from './component-array.vue'

defineProps<{ item: T, index: number }>()
</script>

<template>
  <div>
    <slot :name="item.slot || 'item'" :item="item" :index="index">
      {{ item.label }}
    </slot>
  </div>
</template>
