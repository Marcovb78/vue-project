<template>


  <div class="mb-4 p-8">
    <h1 class="text-4xl font-bold mb-4">Meals by Ingredient</h1>
    <input type="text" 
    v-model="keyword" 
    class="rounded border-2 bg-white border-gray-200 w-full mb-3" 
    placeholder="Search for Ingredients"
    />
  </div>

  <div class="grid grid-cols-1 md:grid-cols-3 gap-5 p-8">
    <router-link 
    :to="{ 
      name: 'byIngredient', 
      params: { ingredient: ingredient.strIngredient },
    }"
      v-for="ingredient of computedIngredients" 
      :key="ingredient.idIngredient" 
      class="bg-white rounded p-8 mb-3 shadow block"
    >
      <h3 class="text-2xl font-bold">{{ ingredient.strIngredient }}</h3>
      <p>{{ ingredient.strDescription }}</p>
    </router-link>
  </div>
  
</template>

<script setup>
import { computed, onMounted, ref } from 'vue';
import axiosClient from '@/axiosClient';

const keyword = ref('');
const ingredients = ref([]);
const computedIngredients = computed(() => {
  if (!computedIngredients) return ingredients;
  return ingredients.value.filter((i) =>
    i.strIngredient.toLowerCase().includes(keyword.value.toLowerCase())
  );
});

onMounted(() => {
  axiosClient.get('list.php?i=list')
    .then(({ data }) => {
      ingredients.value = data.meals;
    })
})

</script>