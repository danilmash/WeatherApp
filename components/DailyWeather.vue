<template>
    <section class="daily-weather">
        <div class="wrapper">
            <h3 class="daily-weather__header section-block-header">Daily weather</h3>
            <div class="daily-weather__container">
                <div class="daily-weather__card">
                    <div class="daily-weather__parameter weather-parameter">
                        <h4 class="weather-parameter__header">Tue</h4>
                        <p class="weather-parameter__description">12/23</p>
                    </div>
                    <img src="~/assets/images/mostly-sunny.svg" alt="" class="daily-weather__icon">
                    <div class="daily-weather__parameter weather-parameter">
                        <h4 class="weather-parameter__header">23°</h4>
                        <p class="weather-parameter__description">High</p>
                    </div>
                    <div class="daily-weather__parameter weather-parameter">
                        <h4 class="weather-parameter__header">23°</h4>
                        <p class="weather-parameter__description">High</p>
                    </div>
                    <div class="daily-weather__parameter weather-parameter">
                        <h4 class="weather-parameter__header">23°</h4>
                        <p class="weather-parameter__description">High</p>
                    </div>
                    <div class="daily-weather__parameter weather-parameter">
                        <h4 class="weather-parameter__header">23°</h4>
                        <p class="weather-parameter__description">High</p>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<script setup>
let props = defineProps(['data'])

const dateFormatter = new Intl.DateTimeFormat('en', {
  weekday: 'short',
})

</script>

<style lang="scss">
    .daily-weather {


        &__card {
            display: flex;
            align-items: center;
            justify-content: space-around;
            margin-top: 15px;
            background-color: #dcdcdc;;
        }

        &__icon {
            max-width: 120px;
            min-width: 70px;
        }
    }
</style>