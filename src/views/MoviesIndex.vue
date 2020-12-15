<template>
  <div class="index">

   <h2>Movies Index</h2>
   <div v-for="movie in movies">
     <router-link v-bind:to="`/movies/${movie.id}`">
      <h3>{{ movie.title }}</h3>
     </router-link>
     <button>more info</button>
   </div>

  </div>
</template>

<style>
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      movies: [],
    };
  },
  
  created: function() {
    this.indexMovies();
  },

  methods: {
    indexMovies: function () {
      axios
        .get("/api/movies")
        .then(response => {
          this.movies = response.data;
          console.log("All Movies", this.movies);
        });
    },
  }
};
</script>