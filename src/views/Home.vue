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
          Search for a movie by it's title.
        </span>
      </div>
    </div>
    <br />
    <button v-on:click="setSortAttribute('title')" class="btn btn-success">
      Sort by title
      <span v-if="sortAttribute === 'title' && sortOrder === 1">^</span>
      <span v-if="sortAttribute === 'title' && sortOrder === -1">v</span>
    </button>

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
            <p class="card-text">Ingredients: {{ movie.ingredients }}</p>
            <p>Plot: {{ movie.plot }}</p>
            <p>Director: {{ movie.director }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
  <!-- <div class="home">
    <h2>Vue Movie App</h2>
    <h3>Add Movie</h3>
    <input type="text" v-model="newTitle" placeholder="Title" />
    <input type="text" v-model="newYear" placeholder="Year" />
    <input type="text" v-model="newPlot" placeholder="Plot" />
    <input type="text" v-model="newDirector" placeholder="Director" />
    <input type="text" v-model="newEnglish" placeholder="English?" />
    <button v-on:click="movieCreate">Add Movie</button>
    <h3>Movie List:</h3>
    <div class="movie" v-for="movie in movies" v-bind:key="movie.id">
      <h4>{{ movie.title }} ({{ movie.year }})</h4>
      <button v-on:click="movieShow(movie)">More Info</button>
    </div>
    <dialog id="movie-info">
      <form method="dialog">
        <h1>Movie Info</h1>
        <p>Title: {{ currentMovie.title }}</p>
        <p>Year: {{ currentMovie.year }}</p>
        <p>Director: {{ currentMovie.director }}</p>
        <p>Plot: {{ currentMovie.plot }}</p>
        <button>Close</button>
      </form>
    </dialog>
  </div> -->
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Vue Movie App",
      movies: [],
      newTitle: "",
      newYear: "",
      newPlot: "",
      newDirector: "",
      newEnglish: "",
      currentMovie: {},
    };
  },
  created: function () {
    this.moviesIndex();
  },
  methods: {
    moviesIndex: function () {
      axios.get("http://localhost:3000/movies").then((response) => {
        this.movies = response.data;
        console.log(response.data);
      });
    },
    movieCreate: function () {
      let params = {
        title: this.newTitle,
        year: this.newYear,
        plot: this.newPlot,
        director: this.newDirector,
        english: this.newEnglish,
      };
      axios
        .post("http://localhost:3000/movies", params)
        .then((response) => {
          this.movies.push(response.data);
          console.log(response.data);
          this.newTitle = "";
          this.newYear = "";
          this.newPlot = "";
          this.newDirector = "";
          this.newEnglish = "";
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },
    movieShow: function (movie) {
      this.currentMovie = movie;
      console.log(movie);
      document.querySelector("#movie-info").showModal();
    },
  },
};
</script>
