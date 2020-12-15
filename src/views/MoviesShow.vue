<template>
  <div class="edit">
    <h2>{{ movie.title }}</h2>
    <p>{{ movie.year }}</p>
    <p>{{ movie.plot }}</p>
    <p>Director: {{ movie.director }}</p>

    <router-link v-bind:to="`/movies/${movie.id}/edit`">
        <button>edit</button>
    </router-link>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      movie: [],
    };
  },

  created: function() {
    axios
      .get("/api/movies/" + this.$route.params.id)
      .then(response => {
        this.movie = response.data;
        console.log("show movie", this.movie);
      });
  }
};
</script>