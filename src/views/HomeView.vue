<script setup>
import { ref, watchEffect, computed, onMounted } from 'vue'
// fetch data from https://dolar-api-argentina.vercel.app/v1/dolares
// const response = await fetch('https://api.bluelytics.com.ar/v2/latest')

const API_URL = 'https://api.bluelytics.com.ar/v2/latest'
const data = ref({})
const date = ref(new Date())
const formatedDate = ref('')
const currencyNames = {
    oficial: 'Dólar Oficial',
    blue: 'Dólar Blue',
    oficial_euro: 'Euro Oficial',
    blue_euro: 'Euro Blue',
}
// Consults API DB
const divisasUpdate = async () => {
    const response = await fetch(API_URL)
    console.log("updated db")
    return data.value = await response.json()
};
// Update data on load
watchEffect(async () => {
  await divisasUpdate()
  divisas.value
  formatSpanishDate.value
})
// Update data every 30 seconds
setInterval(async () => {
    divisasUpdate()
    divisas.value
    formatSpanishDate.value
}, 30000)  
// Get only the first 4 currencies and delete the last_update key
const divisas = computed(() => {
    const dataArray = Object.entries(data.value);
    return Object.fromEntries(dataArray.slice(0, 4));
});
// Format Date to Spanish and Short
const formatSpanishDate = computed(() => {
    date.value = new Date(data.value.last_update);
    const options = { dateStyle: 'medium', timeStyle: 'short' };
    return formatedDate.value = date.value.toLocaleString('es-ES', options);
});
</script>

<template>
  <main>
    <div class="divisas__wrapper">
      <div v-for="(divisa, currencyCode) in divisas" :key="currencyCode" class="divisas__item">
        <h3>{{ currencyNames[currencyCode] }}</h3>
        <div class="divisas__buy">
          <label>Compra:</label>
          <p class="divisas__number">${{ divisa.value_buy }}</p>
        </div>
        <div class="divisas__sell">
          <label>Venta:</label>
          <p class="divisas__number">${{  divisa.value_sell }}</p>
        </div>
      </div>
    </div>
    <div class="last-update">
      <p>Última actualización:</p>
      <p>{{ formatedDate }}</p>
    </div>
  </main>
</template>

<style scoped lang="sass">
.divisas__wrapper
    display: grid
    grid-template-columns: repeat(2, 1fr)
    grid-template-rows: repeat(2, 1fr)
    grid-column-gap: 2rem
    grid-row-gap: 2rem
    padding-top: 2rem
    max-width: 720px
    width: 95%
    margin: 0 auto
    @media (max-width: 570px)
        grid-template-columns: 1fr
        grid-template-rows: repeat(4, 1fr)
        grid-column-gap: 0
        grid-row-gap: 1rem
        max-width: 720px
        width: 90%
        margin: 0 auto
    .divisas__item
        display: grid
        grid-template-columns: 2fr
        grid-template-rows: 1fr 1fr 1fr
        grid-row-gap: 10px
        border: 1px solid lighten(#000, 70%)
        border-bottom: 4px solid lighten(#000, 70%)
        border-radius: 1rem
        line-height: 0.9
        padding: 1rem
        background-color: var(--white)
        box-shadow: 0.2rem 0.3rem 0.8rem rgba(0,0,0,0.2)
        transition: all 0.3s ease
        
        &:hover
          border-color: var(--accent-dark-1)
        h3
            font-size: 2rem
            margin-bottom: 1rem
        .divisas__buy, .divisas__sell
            display: flex
            justify-content: space-between
            width: 100%
        label
            font-weight: 300
            font-size: 1.4rem
        .divisas__number
            font-weight: 600
            font-size: 2rem
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
.last-update
    font-size: 1.2rem
    margin-top: 1rem
    text-align: center
    line-height: 1
    color: var(--accent-dark-3)
    p:first-child
        font-size: 0.8rem
        font-weight: 300
        color: var(--black)
    p:last-child
</style>
