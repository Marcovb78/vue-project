<template>
  <div>
    <img :src="meal.strMealThumb" :alt="meal.strMeal" class="w-full object-cover max-h-[350px]">

    <h1 class="text-5xl font-bold mb-5 mt-5 flex justify-center">{{ meal.strMeal }}</h1>

    <div class="flex flex-row justify-between items-center max-w-[750px] mx-auto mb-5">
      <p> <b>Category:</b> {{ meal.strCategory }}</p> 
      <p> <b>Area:</b> {{ meal.strArea }}</p> 
      <p> <b>Tags:</b> {{ meal.strTags }}</p> 
    </div>

    <div class="flex items-center mx-auto max-w-[1250px] text-center mb-10">
      {{ meal.strInstructions }}
    </div>

    <div class="grid grid-cols-2 sm:grid-cols-2 max-w-[1250px] mx-auto">
      <div>
        <h2 class="text-2xl font-semibold mb-3">Ingredients</h2>
        <ul>
          <template v-for="(el, ind) of new Array(20)">
            <li v-if="meal[`strIngredient${ind + 1}`]">
              {{ ind + 1 }}. {{ meal[`strIngredient${ind + 1}`] }}
            </li>
          </template>
        </ul>
      </div>
      <div>
        <h2 class="text-2xl font-semibold mb-3">Measures</h2>
        <ul>
          <template v-for="(el, ind) of new Array(20)">
            <li v-if="meal[`strMeasure${ind + 1}`]">
              {{ ind + 1 }}. {{ meal[`strMeasure${ind + 1}`] }}
            </li>
          </template>
        </ul>
      </div>
      <div class="mt-10 mb-10">
        <YouTubeButton :href="meal.strYoutube">Go to Youtube</YouTubeButton>
        <a :href="meal.strSource" target="_blank" class="ml-3 px-3 py-2 rounded bg-white text-indigo-400 transition-colors">
          Go to website
        </a>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';
import axiosClient from '@/axiosClient';
import YouTubeButton from '@/components/YouTubeButton.vue';

const route = useRoute();
const meal = ref({})

// API request to get the meal data
onMounted(() => {
  axiosClient.get(`lookup.php?i=${route.params.id}`)
    .then(({ data }) => {
      meal.value = data.meals[0] || {}
    })
})

</script>