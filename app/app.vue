<template>
  <UApp>
    <UHeader>
      <template #title>
        <h1>
          Demo Nested Components
        </h1>
      </template>

      <template #right>
        <ULink to="https://github.com/sandros94/nuxt-nested-components" target="_blank" icon="i-simple-icons-github" />
      </template>
    </UHeader>

    <UMain>
      <NuxtPage />
    </UMain>
  </UApp>
</template>
