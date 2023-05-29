<script setup>
import HelloWorld from "./components/HelloWorld.vue";
import axios from "axios";
import Category from "./components/Category.vue";
import Meal from './components/Meal.vue';
</script>

<template>
  <img
    src="https://live.pystatic.com/webassets/AppscoreWeb/global-page/0.1.1/_next/static/media/peya.bf6a4964.svg"
    class="logo"
    alt="logo app"
  />
  <!-- <HelloWorld msg="Vite + Vue" /> -->
  <!-- <p> Desarrollado por Angel Quezada</p> -->

  <h3>¿Desea buscar una receta?</h3>
  <input
  class="btnbuscar"
    type="text"
    v-model="search"
    v-on:keyup.enter="searchData"
    placeholder="Buscar receta"
  />
  <button v-on:click="chageCategories">Cargar todas las categorias</button>

  <Meal v-for="meal in meals"
v-bind:key="meal.idMeal"
v-bind:meal="meal" />


<div class="text-center">
  ...
</div>

<h3>o Busca por categoria</h3>

<Category
  v-for="category in paginated"
  v-bind:key="category.idCategory"
  v-bind:category="category"
/>

<div class="text-center">
  Actual: {{ current }}
</div>
<div class="text-center">
  <a @click="prev()">Anterior</a>
  |
  <a @click="next()">Siguiente</a>
  
 
</div>
</template>


<script>
export default {
  components: { Category, Meal },
  name: "App",
  data() {
    return {
      categories: [],
      meals: [],
      search: null,
      //Paginacion
      current:1,
      pagesize:5,
    };
  },
  mounted() {
    axios
      .get("https://themealdb.com/api/json/v1/1/categories.php")
      .then((res) => {
        
        this.categories = res.data.categories;
      })
      .catch((err) => {
        console.log(err);
      });
  },
  computed:{

    indexStart(){
      return(this.current -1) * this.pagesize;
    },
    indexEnd(){
      return this.indexStart + this.pagesize;
    },

    paginated(){
      return this.categories.slice(this.indexStart, this.indexEnd);
    }

  },
  methods: {
    searchData() {
      //Verificar si el campo de busqueda tiene texto
      if (this.search != null) {
        axios.get('https://themealdb.com/api/json/v1/1/search.php?s=' + this.search)
        .then((res) =>{
          // idMeal strArea strCategory strMealThumb
          console.log(res.data.meals);
          this.meals = res.data.meals;
          this.categories = null;
        })
        .catch((err) =>{
          console.log(err);
        })
        this.search = null;

      } else if (this.search==null) {
        alert("Campo vacio");
      }
    },
    chageCategories(){
      axios.get('https://themealdb.com/api/json/v1/1/categories.php')
      .then((res)=>{
        this.categories=res.data.categories;
      this.meals=null;

      })
      .catch((err)=>console.log(err))
    }
  },
  prev(){
      this.current--;
    },
    next(){
      this.current++;
    }
};
</script>


<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em orangered);
}

.category-container {
  border: 1px solid yellow;
  padding: 50px;
}

.btnbuscar{
  border-radius: 5px;
  text-align: center;
  font-style: italic;
  font-size: 20px;
}
</style>
