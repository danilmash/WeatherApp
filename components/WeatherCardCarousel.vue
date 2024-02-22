<template>
    <div class="weather-carousel">
        <div class="weather-carousel__cards" ref="scroller">
            <div class="weather-carousel__card" v-for="(card, index) in props.data" :key="index">
                <h4 class="weather-carousel__time">{{ card.title }}</h4>
                <img :src="`https://openweathermap.org/img/wn/${card.icon}@2x.png`" alt="" class="weather-carousel__icon">
                <p class="weather-carousel__temperature">{{(card.description)}}</p>
            </div>
        </div>
        
        <span class="weather-carousel__btn-prev carousel-btn" @click="scrollToLeft()">&lt;</span>
        <span class="weather-carousel__btn-next carousel-btn" @click="scrollToRight()">&gt;</span>
    </div>
</template>

<script setup>
import axios from 'axios';
let props = defineProps(['data'])

let weatherData = ref({})
let scroller = ref(null)

function scrollToRight() {
    scroller.value.scrollBy({left: 80, top: 0, behavior: 'smooth'});
}

function scrollToLeft() {
    scroller.value.scrollBy({left: -80, top: 0, behavior: 'smooth'});
}



</script>

<style lang="scss">
    .weather-carousel {
        position: relative;

        .carousel-btn {
            display: none;
            @media (max-width: 1000px) {
                display: flex;
            }
        }

        &:hover {
            .carousel-btn {
                opacity: 1;
            }
        }

        &__cards {
            display: flex;
            gap: 20px;
            margin-top: 10px;
            padding: 10px 0;
            text-align: center;

            overflow-x: scroll;
            scroll-snap-type: x mandatory;

            &::-webkit-scrollbar {
                display: none;
            }
        }
        
        &__card {
            max-width: 150px;
            flex: 1;

            padding: 10px;
            background-color: #dcdcdc;

            scroll-snap-align: center;
            
        }

        &__time{
            font-size: 1.4rem;
        }

        &__icon {
            min-width: 80px;
        }

        &__temperature {
            font-size: 1.4rem;
        }

        &__btn-prev {
            position: absolute;
        }

        &__btn-next {
            right: 15px;
        }

        &__btn-prev {
            left: 15px;
        }
    }
</style>