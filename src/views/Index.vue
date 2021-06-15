<template>
  <div class="index">
        <input type="text" v-model="searchTerm" placeholder="Search"/>
    <div v-for="movie in filterBy(movies, searchTerm, 'title')" v-bind:key="movie.id">
      <h2>{{ movie.title }}</h2>
      <p>{{ movie.year }}</p>
      <p><strong>Director: </strong>{{ movie.director }}</p>
      <router-link v-bind:to="`/show/${movie.id}`">More Details</router-link>
    </div>
  </div>
</template>

<style>
.more-info {
  margin-bottom: 35px;
}
</style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";
export default {
  mixins: [Vue2Filters.mixin],
  data: function () {
    return {
      movies: [],
      newMovieTitle: "",
      newMovieYear: "",
      newMoviePlot: "",
      newMovieDirector: "",
      newMovieEnglish: "",
      movieDetails: {},
      searchTerm: "",
    };
  },
  created: function () {
    this.indexMovies();
  },
  methods: {
    indexMovies: function () {
      axios.get("/movies").then((response) => {
        console.log(response.data);
        return (this.movies = response.data);
      });
    },
  },
};
</script>