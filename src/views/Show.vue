<template>
  <div class="movies-show">
    <h1>{{ movieDetails.title }}</h1>
    <p><strong>Year:</strong> {{ this.movieDetails.year }}</p>
    <p><strong>Director:</strong> {{ this.movieDetails.director }}</p>
    <p><strong>Plot:</strong> {{ this.movieDetails.plot }}</p>
    <p><strong>English:</strong> {{ this.movieDetails.english }}</p>
    <p><strong>Genres:</strong> {{ this.movieDetails.genre_names }}</p>
    <router-link :to="`/show/${this.movieDetails.id}/edit`"
      >Edit Movie</router-link
    >
    <br />
    <button v-on:click="destroyMovie()">Delete Movie</button>
  </div>
</template>

<style>
.more-info {
  margin-bottom: 35px;
}
button {
  margin-top: 35px;
}
</style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movies: [],
      movieDetails: {},
    };
  },
  created: function () {
    this.showMovie();
  },
  methods: {
    showMovie: function () {
      axios
        .get(`/movies/${this.$route.params.id}`)
        .then((response) => {
          console.log("movie show:", response.data);
          return (this.movieDetails = response.data);
        })
        .catch((error) => {
          console.log(error.response.data);
        });
    },
    destroyMovie: function () {
      if (confirm("Are you sure you want to delete this movie?")) {
        axios.delete(`/movies/${this.$route.params.id}`).then((response) => {
          console.log(response.data);
          this.$router.push("/");
        });
      }
    },
  },
};
</script>