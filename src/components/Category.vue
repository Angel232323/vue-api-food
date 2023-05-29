<template>
  <!-- food container -->
  <div class="category-container">
    <img :src="category.strCategoryThumb" alt="" />
    <h6>{{ category.strCategory }}</h6>
    <p>{{ category.strCategoryDescription }}</p>
  </div>
</template>

<script>
export default {
  props: ["category"],
};
</script>