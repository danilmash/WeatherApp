<template>
    <section class="today-weather">
        <div class="wrapper" v-if="weatherData.name">
            <h2 class="today-weather__location">{{ `${weatherData.name}, ${weatherData.country}` }}</h2>  
            <div class="today-weather__current-weather current-weather">
                <h3 class="current-weather__date section-block-header">{{ dateFormatter.format(weatherData.dt * 1000) }}</h3>
                <div class="current-weather__weather-data">
                    <div class="current-weather__weather">
                        <img class="current-weather__icon" :src="`https://openweathermap.org/img/wn/${weatherData.weather.icon}@2x.png`" alt="Mostly sunny">
                        <div class="current-weather__main">
                            <h4 class="current-weather__temperature">{{ weatherData.temperature }}</h4>
                            <p class="current-weather__description">{{ weatherData.weather.main }}</p>
                        </div>
                    </div>
                    <div class="current-weather__parameters">
                        <div class="current-weather__parameter weather-parameter" v-for="(parameter, index) in weatherData.parameters" :key="index">
                            <h4 class="weather-parameter__header">{{ parameter.value }}</h4>
                            <p class="weather-parameter__description">{{ parameter.description }}</p>
                        </div>
                    </div>
                </div>
            </div>
            <div class="today-weather__brief-cards brief-weather-cards">
                <h3 class="brief-weather-cards__header section-block-header">24 hours weather forecast</h3>
                <WeatherCardCarousel :data="weatherHourlyData"></WeatherCardCarousel>
            </div>
        </div>
    </section>
</template>

<script setup>
import axios from 'axios';

let props = defineProps(["data"]); 

let timeFormatter = new Intl.DateTimeFormat('en', {
  hour: '2-digit',
  minute: '2-digit',
  hour12: false,
});

const dateFormatter = new Intl.DateTimeFormat('en', {
  weekday: 'long',
  day: 'numeric',
  month: 'long'
})

let weatherHourlyData = ref([])

async function getWeatherHourly() {
    let data = null
    await axios.get('https://api.openweathermap.org/data/2.5/forecast', {
        params: {
            lon: weatherData.value.lon, 
            lat: weatherData.value.lat,
            cnt: 8,
            units: 'metric',
            appid: 'c2c875b5089905de1a3cb5cfe8d0f621',
        }
    }).then((response) => {
        data = response.data
        weatherHourlyData.value = []
        for (let weather of data.list) {
        weatherHourlyData.value.push({
            title: weather.dt_txt.slice(10, 16),
            icon: weather.weather[0].icon,
            description: Math.round(weather.main.temp) + '°'
        })
    }
    })
    
}

let weatherData = ref({})

function formatWeatherData() {
    weatherData.value = {
        name: props.data.name,
        country: props.data.sys.country,
        lon: props.data.coord.lon,
        lat: props.data.coord.lat,
        dt: props.data.dt,
        weather: props.data.weather[0],
        temperature: Math.round(props.data.main.temp) + "°",
        parameters: {
            max_temperature: {
                value: Math.round(props.data.main.temp_max) + "°",
                description: 'High'
            },
            min_temperature: {
                value: Math.round(props.data.main.temp_min) + "°",
                description: 'Low'
            },
            humidity: {
                value: props.data.main.humidity,
                description: 'Humidity'
            },
            wind: {
                value: props.data.wind.speed + " m/s",
                description: 'Wind'
            },
            sunrise: {
                value: timeFormatter.format(props.data.sys.sunrise * 1000),
                description: 'Sunrise'
            },
            sunset: {
                value: timeFormatter.format(props.data.sys.sunset * 1000),
                description: 'Sunset'
            }
        }
    }
}
formatWeatherData()
await getWeatherHourly() 
onBeforeUpdate(async () => {
    formatWeatherData()
    await getWeatherHourly() 
})




</script>

<style lang="scss">
    .today-weather {
        padding: 30px 0;

        &__location {
            font-size: 2rem;
            font-weight: 600;
        }
    }

    .current-weather {

        &__header {

        }

        &__weather-data {
            position: relative;
            display: flex;
            flex-wrap: wrap;
            justify-content: space-evenly;
            padding: 30px 0;
            gap: 50px;

            &::before {
                content: '';
                position: absolute;
                top: 0;
                bottom: 0;
                width: 3px;
                background-color: rgb(139, 139, 139);
                left: 50%;
                transform: translateX(-50%);
                
                @media (max-width: 683px) {
                    top: 55%;
                    bottom: 0;
                    left: 0;
                    transform: translateY(-50%);
                    width: 100%;
                    height: 3px;
                }
            }
        }

        &__weather {
            display: flex;
            flex-grow: 1;
            max-width: 350px;
            align-items: center;
            gap: 10px;
        }

        &__icon {
            min-width: 150px;
        }

        &__main {
        }

        &__temperature {
            font-size: 6rem;
            line-height: 80%;
        }

        &__description {
            font-size: 1.3rem;
            margin-top: 5px;
        }

        &__parameters {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(103px, 1fr));
            flex-wrap: wrap;
            flex: 1;
            align-items: center;
            justify-content: center;
            row-gap: 20px;
            max-width: 350px;
            
            text-align: center;
        }
    }

    .weather-parameter {

        &__header {
            font-size: 1.7rem;
        }

        &__description {
            font-size: 1.2rem;
        }
    }
      
    .brief-weather-cards {
       padding: 30px 0;
    }

</style>