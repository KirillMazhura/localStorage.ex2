<script setup>
import { RouterLink, RouterView } from 'vue-router'
import {createApp, ref, onMounted} from 'vue'
import axios from 'axios'

const weather = ref({})
const cities = ref([])
const city = ref('')
const choosenCity= ref('')
onMounted(()=> {
  axios.get(`https://api.openweathermap.org/data/2.5/weather?q=Zaporizhia&appid=7914d5a440960cfd5df3bd0388a7ad0f`)
  .then(response => {
    weather.value = response.data.main
  })
});
// function addCity() {
//   if (city) {
//     cities.value.push(city)
//   }
// }
const addCity= () => {
  if (city) {
    cities.value.push(city.value)
  }
}
const getWeather = () => {
  if (choosenCity) {
    axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${choosenCity}&appid=7914d5a440960cfd5df3bd0388a7ad0f`)
  .then(response => {
    weather.value = response.data.main
  })
  .catch(error => {
    // Handle any errors
    console.log(error);
  });
  }
}

</script>

<template>
  <header>
    <div class="logoBlock">
      <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" />
    </div>
    {{ weather }}
    <h1>-----</h1>
    {{ cities }}
    <h1>-----</h1>
    {{ choosencity }}
    <h1>{{ url }}</h1>
    <div class="wrapper">
      <div class="city-choose">
        <h2>Choose city</h2>
        <select v-model="choosencity">
          <option disabled>Choose city</option>
          <option v-for="c in cities" v-bind:key="c">{{ c }}</option>
        </select>
        <button @click="getWeather">Look for weather</button>
      </div>
      <div class="city-add">
        <input type="text" v-model="city">
        <button @click="addCity">Add city</button>
      </div>
      <nav>
        <!-- <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink> -->
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
  header {
    margin: auto 0
  }
  .logoBlock {
    margin: 0 auto;
    width: 125px;
    height: 125px;
  }
  .wrapper {
    margin-top: 50px;
    display: flex;
    justify-content: space-between;
  }
  .city-add {
    display: flex;
    flex-direction: column;
    margin-bottom: 10px;
  }
</style>
