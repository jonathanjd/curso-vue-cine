<template>
  <v-container grid-list-xs>
    <v-layout row wrap class="mt-4 mb-4">
      <template v-for="(movie, index) in movies">
        <v-flex xs4>
          <v-card class="mx-2 mb-1">
            <v-card-media class="white--text" height="200px" :src="movie.Poster">
              <v-container fill-height fluid>
                <v-layout fill-height>
                  <v-flex xs12 align-end flexbox>
                    <span class="headline">{{ movie.Title }}</span>
                  </v-flex>
                </v-layout>
              </v-container>
            </v-card-media>
            <v-card-title>
              <div>
                <span class="grey--text">Año: {{ movie.Year }}</span>
                <br>
                <span>Título: {{ movie.Title }}</span>
              </div>
            </v-card-title>
            <v-card-actions>
              <v-btn flat color="primary">Compartir</v-btn>
              <v-btn flat color="purple" @click="showMovie">Más Información</v-btn>
            </v-card-actions>
          </v-card>
        </v-flex>
      </template>
    </v-layout>

    <!--  *** My Modal Start *** -->
    <v-dialog v-model="showModal" max-width="500px">
      <v-card>
        <v-card-title>
          Dialog 2
        </v-card-title>
        <v-card-text>
          <p>Texto</p>
        </v-card-text>
        <v-card-actions>
          <v-btn color="primary" flat @click.stop="showModal = false">Close</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!--  *** My Modal End *** -->

  </v-container>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      movies: [],
      showModal: false
    };
  },

  created() {
    this.moviesDefault();
  },

  methods: {
    showMovie() {
      this.showModal = true;
    },
    moviesDefault() {
      axios.get('http://www.omdbapi.com/?apikey=c53bd7a7&s=2016').then(response => {
        console.log(response.data);
        this.movies = response.data.Search;
      });
    }
  }
};
</script>

<style>
</style>
