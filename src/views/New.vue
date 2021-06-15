<template>
  <div class="movies-new">
    <div>
      Title:
      <input type="text" v-model="newMovieTitle" placeholder="Title" />
      <br />
      Year:
      <input type="text" v-model="newMovieYear" placeholder="Year" />
      <br />
      Director:
      <input type="text" v-model="newMovieDirector" placeholder="Director" />
      <br />
      Plot:
      <textarea name="plot" cols="30" rows="10" maxlength="300" v-model="newMoviePlot"></textarea>
      <br />
      <p>{{ 300 - newMoviePlot.length }} characters remaining</p>
      English?
      <br />
      <div>
        <label for="true">
          True
          <input value="true" type="radio" id="englishRadio" name="english" v-model="newMovieEnglish" />
        </label>
        <br />

        <label for="false">
          False
          <input value="false" type="radio" id="englishRadio" name="english" v-model="newMovieEnglish" />
        </label>
        <br />
      </div>

      <button class="more-info" v-on:click="createMovie">Create New Movie</button>
    </div>
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
