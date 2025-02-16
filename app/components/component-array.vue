<!-- eslint-disable vue/block-tag-newline -->
<script lang="ts">
export interface Item {
  label: string
  myParam?: string
  myOtherParam?: string
  slot?: string
}

export interface ComponentProps<T extends NestedArray<Item>> {
  items: T
}

type SlotProps<T extends Item> = (props: { item: T, index: number, active?: boolean }) => any
export type ComponentSlots<T extends NestedArray<Item>> = {
  'item': SlotProps<NestedItem<T>>
  'item-content': SlotProps<NestedItem<T>>
} & DynamicSlots<NestedItem<T>, SlotProps<NestedItem<T>>>

type DynamicSlots<T extends { slot?: string }, SlotProps, Slot = T['slot']> =
  Record<string, SlotProps> & (Slot extends string ? Record<Slot, SlotProps> : Record<string, never>)
type NestedArray<T> = T extends Array<infer I> ? I[] | I[][] : T[] | T[][]
type NestedItem<T> = T extends Array<infer I> ? NestedItem<I> : T

</script>

<!-- eslint-disable import/first -->
<script setup lang="ts" generic="T extends NestedArray<Item>">
import { createReusableTemplate } from '@vueuse/core'

const props = defineProps<ComponentProps<T>>()
defineSlots<ComponentSlots<T>>()

const lists = computed<Item[][]>(() =>
  props.items?.length
    ? isArrayOfArray(props.items)
      ? props.items
      : [props.items]
    : [],
)

function isArrayOfArray<A>(item: A[] | A[][]): item is A[][] {
  return Array.isArray(item[0])
}

const [Define, Reuse] = createReusableTemplate<{ item: Item, index: number }, ComponentSlots<T>>()
</script>

<template>
  <Define v-slot="{ item, index }">
    <slot :name="item.slot || 'item'" :item="(item as NestedItem<T>)" :index="index" />
  </Define>

  <div>
    <template v-for="(list, listIndex) in lists" :key="`list-${listIndex}`">
      <Reuse v-for="(item, itemIndex) in list" :key="`item-${itemIndex}`" :item="item" :index="itemIndex" />
    </template>
  </div>
</template>
