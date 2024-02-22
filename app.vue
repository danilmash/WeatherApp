<template>
  <main>
    <div class="wrapper">
      <div class="search">
        <label class="search__label" for="search">
          <img class="search__icon" src="~/assets/images/search.svg" alt="Search">
        </label>
        <input v-model="searchQuery" @input="startSearch" id="search" class="search__input" type="text">
        <div class="search__results">
          <span class="loader" v-show="loading"></span>
          <div class="search__result search-result" v-for="(result, index) in searchResults" :key="index" @mousedown="startSearchCurrentWeatherData(result)">
            <h2 class="search-result__header">{{ `${result.name}, ${result.state}, ${result.country}` }}</h2>
            <p class="search-result__description"></p>
          </div>
        </div>
      </div>
    </div>
    <CurrentWeather :data="currentWeatherData"></CurrentWeather>
  </main>
</template>

<script setup>
import axios from 'axios';

let loading = false
let searchQuery = ref('')
let searchResults = ref({})

async function startSearch() {
  loading = true
  searchResults.value = {}
  if (searchQuery.value != '') {
    searchResults.value = await search()
  }
  loading = false
}

async function search() {
  let data = null
  await axios.get('http://api.openweathermap.org/geo/1.0/direct', {
    params: {
        q: searchQuery.value,
        appid: 'c2c875b5089905de1a3cb5cfe8d0f621',
        limit: 5,
      }
    }
  ).then((response) => {
    data = response.data 
  }).catch((error) => {
    data = error
  })
  return data
  
}

let currentWeatherData = ref({})
let dailyWeatherData = ref({})

async function startSearchCurrentWeatherData(city) {
  let lon = city.lon
  let lat = city.lat
  await getCurrentWeatherData(lon, lat)
}

async function getCurrentWeatherData(lon, lat) {
    let data = null
    await axios.get('https://api.openweathermap.org/data/2.5/weather', {
        params: {
            lat: lat,
            lon: lon,
            appid: 'c2c875b5089905de1a3cb5cfe8d0f621', 
            units: 'metric',
        }
    }).then((response) => {
        data = response.data
        currentWeatherData.value = data
    })
}

async function getDailyWeatherData(lon, lat) {
    let data = null
    await axios.get('https://api.openweathermap.org/data/2.5/forecast/daily', {
        params: {
            lat: lat,
            lon: lon,
            cnt: 5,
            appid: 'c2c875b5089905de1a3cb5cfe8d0f621', 
            units: 'metric',
        }
    }).then((response) => {
        data = response.data
        currentWeatherData.value = data
    })
}

await getCurrentWeatherData(37.6174943, 55.7504461)
</script>

<style lang="scss">
  .search {
    position: relative;
    display: block;
    margin: 0 auto;
    margin-top: 30px;
    width: 100%;
    max-width: 500px;

    &__input {
      width: 100%;
      padding: 10px;
      padding-right: 40px;
      background-color: #cecece;
      font-size: 1.3rem;

      &:focus {
        &+div {
          display: block;
        }
      }

      &+div {
        display: none;
      }

    }

    &__label {
      position: absolute;
      width: 30px;
      height: 30px;
      top: 50%;
      right: 5px;
      translate: 0 -50%;
    }

    &__results {
      position: absolute;
      z-index: 1;
      top: 100%;
      width: 100%;
      background-color: #dcdcdc;
    }

    &__result{
      padding: 10px;
      font-size: 1.2rem;
    }
  }

  

</style>
