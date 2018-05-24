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
              <v-btn flat color="purple" @click="showMovie(movie.imdbID)">Más Información</v-btn>
            </v-card-actions>
          </v-card>
        </v-flex>
      </template>
    </v-layout>

    <!--  *** My Modal Start *** -->
    <v-dialog v-model="showModal" max-width="500px">
      <v-card>
        <template v-if="loading">
          <v-card-title>
            <h3>Cargando...</h3>
          </v-card-title>
          <v-card-text class="text-xs-center">
            <v-progress-circular :size="70" :width="7" indeterminate color="purple"></v-progress-circular>
          </v-card-text>
          <v-card-actions>
            <v-btn color="primary" flat @click.stop="closeModal">Cerrar</v-btn>
          </v-card-actions>
        </template>
        <template v-else>
          <v-card-media class="white--text" :src="modalInfo.poster" height="200px">
            <v-container fill-height fluid>
              <v-layout fill-height>
                <v-flex xs12 align-end flexbox>
                  <span class="headline">{{ modalInfo.title }}</span>
                </v-flex>
              </v-layout>
            </v-container>
          </v-card-media>
          <v-card-title>
            <h3>{{ modalInfo.title }}</h3>
          </v-card-title>
          <v-card-text>
            <p>
              <strong>Año:</strong>{{ modalInfo.age }}</p>
            <p>
              <strong>Min:</strong>{{ modalInfo.min }}</p>
            <p>
              <strong>Género:</strong>{{ modalInfo.genre }}</p>
            <p>
              <strong>Director:</strong>{{ modalInfo.director }}</p>
            <p>
              <strong>Actores:</strong>{{ modalInfo.actors }}</p>
          </v-card-text>
          <v-card-actions>
            <v-btn color="primary" flat @click.stop="closeModal">Cerrar</v-btn>
          </v-card-actions>
        </template>
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
      showModal: false,
      loading: false,
      modalInfo: {
        title: '',
        age: '',
        min: '',
        genre: '',
        director: '',
        poster: ''
      }
    };
  },

  created() {
    this.moviesDefault();
  },

  methods: {
    closeModal() {
      this.showModal = false;
      this.modalInfo.title = '';
      this.modalInfo.age = '';
      this.modalInfo.min = '';
      this.modalInfo.genre = '';
      this.modalInfo.director = '';
      this.modalInfo.poster = '';
      this.modalInfo.actors = '';
    },

    showMovie(id) {
      this.showModal = true;
      this.loading = true;
      axios
        .get('http://www.omdbapi.com/?apikey=c53bd7a7&i='.concat(id))
        .then(response => {
          console.log(response.data);
          this.modalInfo.title = response.data.Title;
          this.modalInfo.age = response.data.Year;
          this.modalInfo.min = response.data.Runtime;
          this.modalInfo.genre = response.data.Genre;
          this.modalInfo.director = response.data.Director;
          this.modalInfo.poster = response.data.Poster;
          this.modalInfo.actors = response.data.Actors;
          this.loading = false;
        })
        .catch(error => {
          console.log(Error);
          this.showModal = false;
          this.loading = false;
        });
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
