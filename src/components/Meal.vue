<template>
  <!-- food container -->
  <div class="meal-container">   
    <h2>{{meal.strMeal}}</h2> 
    <img :src="meal.strMealThumb" alt="" />
    <h5>Categoria: {{meal.strCategory}} | Origen: {{ meal.strArea }} </h5>
  </div>
</template>

<script>
export default {
  props: ["meal"],
};
</script>