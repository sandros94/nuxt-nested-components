<!-- eslint-disable vue/block-tag-newline -->
<script lang="ts">
export interface Item {
  label: string
  myParam?: string
  myOtherParam?: string
  slot?: string
}

export interface ComponentProps<T extends Item | Item[]> {
  items: T[]
}

type SlotProps<T extends Item> = (props: { item: T, index: number, active?: boolean }) => any
export type NavigationMenuSlots<T extends Item> = {
  'item': SlotProps<T>
  'item-content': SlotProps<T>
} & DynamicSlots<T, SlotProps<T>>

export type DynamicSlots<T extends { slot?: string }, SlotProps, Slot = T['slot']> =
  Record<string, SlotProps> & (Slot extends string ? Record<Slot, SlotProps> : Record<string, never>)

</script>

<!-- eslint-disable import/first -->
<script setup lang="ts" generic="T extends Item | Item[]">
import ComponentArrayItem from './component-array-item.vue'

const props = defineProps<ComponentProps<T>>()
defineSlots<NavigationMenuSlots<T extends Array<Item> ? T[number] : T>>()

const lists = computed(() =>
  props.items?.length
    ? isArrayOfArray(props.items)
      ? props.items
      : [props.items]
    : [],
)

function isArrayOfArray<A>(item: A[] | A[][]): item is A[][] {
  return Array.isArray(item[0])
}
</script>

<template>
  <div>
    <template v-for="(list, listIndex) in lists" :key="`list-${listIndex}`">
      <ComponentArrayItem v-for="(item, itemIndex) in list" :key="`item-${itemIndex}`" :item="(item as T extends Array<Item> ? T[number] : T)" :index="itemIndex" />
    </template>
  </div>
</template>
