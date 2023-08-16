<script setup>
import { RouterLink, RouterView } from 'vue-router'


</script>

<template>
  <header>
  <h1>Dólar Hoy</h1>
  </header>
  <main>
    <Suspense>
      <RouterView />
    </Suspense>
  </main>
</template>

<style scoped lang="sass">
body
    background-color: var(--darkGreen)
</style>
