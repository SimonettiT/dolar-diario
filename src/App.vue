<script setup>
import { RouterLink, RouterView } from 'vue-router'


</script>

<template>
  <header>
  <h1>DólarDiario</h1>
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
header
    display: flex
    justify-content: center
h1
    font-weight: 600
    position: relative
    display: inline-block
    font-size: 3rem
    &::after
        content: ''
        position: absolute
        display: block
        bottom: 5px
        right: -10px
        width: 5rem
        height: 0.3rem
        background-color: var(--accent-dark-1)
        border-radius: 50px
</style>
