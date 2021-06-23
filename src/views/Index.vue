<template>
  <div class="movies-index">
    <datalist id="titles">
      <option v-for="movie in movies" v-bind:key="movie.id">
        {{ movie.title }}
      </option>
    </datalist>

    <div class="row g-3 align-items-center">
      <div class="col-auto">
        <label for="inputPassword6" class="col-form-label">Search</label>
      </div>
      <div class="col-auto">
        <input
          type="text"
          id="inputPassword6"
          class="form-control"
          aria-describedby="searchBox"
          v-model="searchTerm"
          list="titles"
        />
      </div>
      <div class="col-auto">
        <span id="searchBox" class="form-text">
        </span>
      </div>
    </div>
    <br />
    <!-- <button v-on:click="setSortAttribute('title')" class="btn btn-success">
      Sort by title
      <span v-if="sortAttribute === 'title' && sortOrder === 1">^</span>
      <span v-if="sortAttribute === 'title' && sortOrder === -1">v</span>
    </button> -->

    <div class="row row-cols-1 row-cols-md-3 g-4">
      <div
        class="col"
        v-for="movie in filterBy(
          orderBy(movies, sortAttribute, sortOrder),
          searchTerm
        )"
        v-bind:key="movie.id"
      >
        <div class="card">
          <router-link :to="`/show/${movie.id}`">
            <img
              :src="movie.image_url"
              class="card-img-top"
              :alt="movie.title"
            />
          </router-link>
          <div class="card-body">
            <h5 class="card-title">{{ movie.title }}</h5>
            <p>Plot: {{ movie.plot }}</p>
            <p>Director: {{ movie.director }}</p>
          </div>
        </div>
      </div>
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
  // setSortAttribute: function (attribute) {
  //     if (this.sortAttribute === attribute) {
  //       this.sortOrder = this.sortOrder * -1;
  //     } else {
  //       this.sortOrder = 1;
  //       this.sortAttribute = attribute;
  //     }
  //    }
};
</script>