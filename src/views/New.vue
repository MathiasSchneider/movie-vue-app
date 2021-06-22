<template>
  <div class="movies-new">
    <form method="POST" action="/new" v-on:submit.prevent="createMovie()">
      <div>
        <label for="title">Title:</label>
        <input type="text" name="title" id="title" v-model="newMovieTitle" placeholder="Title" />
        <br />
        <label for="year">Year:</label>
        <input type="text" name="year" id="year" v-model="newMovieYear" placeholder="Year" />
        <br />
        <label for="director">Director:</label>
        <input type="text" name="director" id="director" v-model="newMovieDirector" placeholder="Director" />
        <br />
        <label for="plot">Plot:</label>
        <textarea
          type="textarea"
          name="plot"
          id="plot"
          cols="30"
          rows="10"
          maxlength="300"
          v-model="newMoviePlot"
          placeholder="Plot summary"
        ></textarea>
        <br />
        <p>{{ 300 - newMoviePlot.length }} characters remaining</p>
        <label for="english">English?</label>
        <br />
        <div>
          <label for="true">
            True
            <input type="radio" name="englishTrue" id="english" value="true" v-model="newMovieEnglish" />
          </label>
          <br />

          <label for="false">
            False
            <input type="radio" name="englishFalse" id="english" value="false" v-model="newMovieEnglish" />
          </label>
          <br />
        </div>

        <input type="submit" class="btn btn-primary" value="Create New Movie" />
      </div>
    </form>
  </div>
</template>

<style>
.more-info {
  margin-bottom: 35px;
}
input {
  margin-bottom: 5px;
}
</style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movies: [],
      newMovieTitle: "",
      newMovieYear: "",
      newMoviePlot: "",
      newMovieDirector: "",
      newMovieEnglish: "",
    };
  },
  created: function () {},
  methods: {
    createMovie: function () {
      let params = {
        title: this.newMovieTitle,
        year: this.newMovieYear,
        plot: this.newMoviePlot,
        director: this.newMovieDirector,
        english: this.newMovieEnglish,
      };
      axios
        .post("http://localhost:3000/movies", params)
        .then((response) => {
          console.log(response.data);
          this.movies.push(response.data);
          this.$router.push("/");
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },
  },
};
</script>
