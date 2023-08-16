<script setup>
import { ref, watchEffect, computed } from 'vue'
// fetch data from https://dolar-api-argentina.vercel.app/v1/dolares
// const response = await fetch('https://api.bluelytics.com.ar/v2/latest')

const API_URL = 'https://api.bluelytics.com.ar/v2/latest'
const data = ref({})

const currencyNames = {
  oficial: 'Dólar Oficial',
  blue: 'Dólar Blue',
  oficial_euro: 'Euro Oficial',
  blue_euro: 'Euro Blue',
}


watchEffect(async () => {
  const response = await fetch(API_URL)
  data.value = await response.json()
})

const divisas = computed(() => {
  const dataArray = Object.entries(data.value);
  return Object.fromEntries(dataArray.slice(0, 4));
});
console.log(data.value)

function formatSpanishDate(timestamp) {
  const date = new Date(timestamp);
  const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric', hour: 'numeric', minute: 'numeric', second: 'numeric', timeZoneName: 'short' };
  return date.toLocaleString('es-ES', options);
}
</script>

<template>
  <main>
    <div class="divisas__wrapper">
      <div v-for="(divisa, currencyCode) in divisas" :key="currencyCode" class="divisas__item">
        <h3>{{ currencyNames[currencyCode] }}</h3>
        <div class="divisas__buy">
          <p>Compra:</p>
          <p>{{ divisa.value_buy }}</p>
        </div>
        <div class="divisas__sell">
          <p>Venta:</p>
          <p>{{  divisa.value_sell }}</p>
        </div>
        <div class="divisas__fluctuation">
          <h5>Fluctuación</h5>
          <p>{{ divisa.last_update }}</p>
        </div>
      </div>
      {{ formatSpanishDate(data.last_update) }}
    </div>
  </main>
</template>

<style scoped lang="sass">
.divisas__wrapper
    display: flex
    flex-wrap: wrap
    justify-content: center
    margin-top: 2rem
    max-width: 1000px
    width: 95%
    margin: 0 auto
    .divisas__item
        display: grid
        grid-template-columns: 2fr
        grid-template-rows: 1fr 1fr 1fr
        width: 20rem
        height: 20rem
        margin: 1rem
        border: 1px solid #000
        border-radius: 1rem
        padding: 1rem
        background-color: var(--white)
        box-shadow: 1rem 1rem 1rem rgba(0,0,0,0.2)
        h3
            font-size: 2rem
            margin-bottom: 1rem
        .divisas__buy
            display: flex
            justify-content: space-between
            width: 100%
            p
                font-size: 1.5rem
        .divisas__sell
            display: flex
            justify-content: space-between
            width: 100%
            p
                font-size: 1.5rem
        .divisas__fluctuation
            display: flex
            flex-direction: column
            align-items: center
            justify-content: center
            width: 100%
            h5
                font-size: 1.5rem
                margin-bottom: 1rem
            p
                font-size: 1.5rem
</style>
