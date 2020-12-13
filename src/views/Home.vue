<template>
  <div class="home">
    <h1>Welcome Back!</h1>

    <h2>Create a New Movie</h2>
    <div>
      Title: <input type="text" v-model="newMovieTitle">
      Year: <input type="text" v-model="newMovieYear">
      Plot: <input type="text" v-model="newMoviePlot">
      Director: <input type="text" v-model="newMovieDirector">
    </div>
    <button v-on:click="createMovie()">create</button>

   <h2>Movies Index</h2>
   <div v-for="movie in movies">
     <h3>{{ movie.title }}</h3>
     <p>{{ movie.year }}</p>
     <button v-on:click="showMovie(movie)">more info</button>
   </div>

  <dialog id="movie-details">
    <form method="dialog" >
      <p>Title: <input type="text" v-model="currentMovie.title"></p>
      <p>Year: <input type="text" v-model="currentMovie.year"></p>
      <p>Plot: <input type="text" v-model="currentMovie.plot"></p>
      <p>Director: <input type="text" v-model="currentMovie.director"></p>
      <button v-on:click="updateMovie(currentMovie)">create</button>
      <button v-on:click="destroyMovie(currentMovie)">delete</button>
      <button>close</button>
    </form>
  </dialog>

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
      currentMovie: {},
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

    showMovie: function(movie) {
      this.currentMovie = movie;
      document.querySelector("#movie-details").showModal();
    },

    updateMovie: function(movie) {
      var params = {
        title: movie.title,
        year: movie.year,
        plot: movie.plot,
        director: movie.director,
      };
      axios
        .patch("/api/movies/" + movie.id, params)
        .then(response => {
          console.log("successfully updated!", response.data);
        });
    },

    destroyMovie: function(movie) {
      axios
        .delete("/api/movies/" + movie.id)
        .then(response => {
          console.log("deleted!", response.data);
          var index = this.movies.indexOf(movie);
          this.movies.splice(index, 1);
        });
    },
  }
};
</script>