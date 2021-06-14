<template>
  <div class="movies-edit">
    <div>
      Title:
      <input
        type="text"
        v-model="editMovieParams.title"
        placeholder="Title"
      /><br />
      Year:
      <input
        type="text"
        v-model="editMovieParams.year"
        placeholder="Year"
      /><br />
      Director:
      <input
        type="text"
        v-model="editMovieParams.director"
        placeholder="Director"
      /><br />
      Plot:
      <textarea
        name="plot"
        cols="30"
        rows="10"
        v-model="editMovieParams.plot"
      ></textarea
      ><br />

      English? <br />
      <div>
        <label for="true"
          >True
          <input
            value="true"
            type="radio"
            id="englishRadio"
            name="english"
            v-model="editMovieParams.english" /></label
        ><br />

        <label for="false"
          >False
          <input
            value="false"
            type="radio"
            id="englishRadio"
            name="english"
            v-model="editMovieParams.english"
          />
        </label>
        <br />
      </div>

      <button class="more-info" v-on:click="editMovie()">Save</button>
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
export default {
  data: function () {
    return {
      movies: [],
      editMovieParams: {},
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
          return (this.editMovieParams = response.data);
        })
        .catch((error) => {
          console.log(error.response.data);
        });
    },
    editMovie: function () {
      axios
        .patch(`movies/${this.editMovieParams.id}`, this.editMovieParams)
        .then((response) => {
          console.log(response.data);
          this.$router.push(`/show/${response.data.id}`);
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },
  },
};
</script>