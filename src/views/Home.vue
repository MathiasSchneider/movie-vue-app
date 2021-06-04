<template>
  <div class="home">
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
  </div>
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
