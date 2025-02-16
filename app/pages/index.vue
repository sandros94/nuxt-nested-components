<template>
  <UContainer class="grid grid-cols-2 gap-2 mt-4">
    <UCard class="w-full">
      <ComponentArray :items="items">
        <template #item="{ item }">
          <div>{{ item.label }}</div>
        </template>
      </ComponentArray>
    </UCard>
    <UCard class="w-full">
      <ComponentArray :items="nestedItems">
        <template #item="{ item }">
          <div>{{ item.label }}</div>
        </template>
      </ComponentArray>
    </UCard>
  </UContainer>
</template>

<script setup lang="ts">
const items = [
  {
    label: 'Item 1',
    myParam: 'param1',
  },
  {
    label: 'Item 2',
    myOtherParam: 'param2',
  },
]
const nestedItems = [
  [
    {
      label: 'Item 1',
      myParam: 'param1',
    },
    {
      label: 'Item 2',
      myOtherParam: 'param2',
    },
  ],
  [
    {
      label: 'Item 3',
      myParam: 'param3',
    },
    {
      label: 'Item 4',
      myOtherParam: 'param4',
    },
  ],
]
</script>
