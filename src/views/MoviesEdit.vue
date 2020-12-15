<template>
  <div class="update">

    <h2>Edit</h2>
    <div>
      Title: <input type="text" v-model="movie.title"><br>
      Year: <input type="text" v-model="movie.year"><br>
      Plot: <input type="text" v-model="movie.plot"><br>
      Director: <input type="text" v-model="movie.director">
    </div>
    <button v-on:click="updateMovie(movie)">create</button>
    <button v-on:click="destroyMovie(movie)">delete</button>

  </div>
</template>

<style>
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      movie: {},
    };
  },
  
  created: function() {

  },

  methods: {
    updateMovie: function(movie) {
      var params = {
        title: this.movie.title,
        year: this.movie.year,
        plot: this.movie.plot,
        director: this.movie.director,
      };
      axios
        .patch("/api/movies/" + this.$route.params.id, params)
        .then(response => {
          console.log("successfully updated!", response.data);
        });
    },

    destroyMovie: function(movie) {
      axios
        .delete("/api/movies/" + this.$route.params.id)
        .then(response => {
          console.log("deleted!", response.data);
          var index = this.movie.indexOf(movie);
          this.movie.splice(index, 1);
        });
    },
  }
};
</script>