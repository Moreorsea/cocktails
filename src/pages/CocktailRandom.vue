<script setup>
import AppLayout from '../components/AppLayout.vue'
import {computed, ref} from 'vue'
import { useRootStore } from '@/stores/root'
import {RANDOM_COCKTAIL, INGREDIENT_PIC} from '../constants'
import { Swiper, SwiperSlide } from 'swiper/vue';
import 'swiper/css';
import axios from 'axios'

const rootStore = useRootStore()
const cocktail = ref(null)

const ingredients = computed(() => {
  const ingredients = []

  for(let i = 1; i <= 15; i++) {
    if(!cocktail.value[`strIngredient${i}`] ) {
      break;
    }

    const ingredient = cocktail.value[`strIngredient${i}`]
    ingredients.push(ingredient)
  }

  return ingredients
})

async function getCocktail() {
  const data = await axios.get(RANDOM_COCKTAIL)
  cocktail.value = data?.data?.drinks[0]
}

getCocktail()

</script>

<template>
  <div class="wrap" v-if="cocktail">
    <AppLayout :imgUrl="cocktail.strDrinkThumb">
    <div class="wrapper">
      <div class="info">
        <div class="title">{{cocktail.strDrink}}</div>
        <div class="line"></div>
        <div class="slider">
          <swiper
            :slides-per-view="3"
            :space-between="50"
            class="swiper"
          >
            <swiper-slide v-for="(ingredient, key) in ingredients" :key="key">
              <img :src="`${INGREDIENT_PIC}${ingredient}-Small.png`" />
              <div class="name">{{ingredient}}</div>
            </swiper-slide>
          </swiper>
        </div>
        <div class="instructions">{{cocktail.strInstructions}}</div>
      </div>
    </div>
    </AppLayout>
  </div>
</template>

<style lang="sass" scoped>
@import '../assets/styles/main'

.slider
  padding: 50px 0

.swiper
  width: 586px

  &-slide
    display: flex
    flex-direction: column

.name
  margin-top: 20px
</style>