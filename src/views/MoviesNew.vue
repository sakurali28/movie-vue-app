<template>
  <div class="new">

    <h2>Create a New Movie</h2>
    <div>
      Title: <input type="text" v-model="newMovieTitle"><br>
      Year: <input type="text" v-model="newMovieYear"><br>
      Plot: <input type="text" v-model="newMoviePlot"><br>
      Director: <input type="text" v-model="newMovieDirector">
    </div>
    <button v-on:click="createMovie()">create</button>

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
      newMovieTitle: "",
      newMovieYear: "",
      newMoviePlot: "",
      newMovieDirector: "",
    };
  },
  
  created: function() {

  },

  methods: {
    
    createMovie: function() {
      var params = {
        title: this.newMovieTitle,
        year: this.newMovieYear,
        plot: this.newMoviePlot,
        director: this.newMovieDirector,
      };
      axios
        .post("/api/movies", params)
        .then(response => {
          console.log("created successfully!", response.data);
          this.movies.push(response.data);
        })
        .catch(error => console.log(error.response));
    },

  }
};
</script>