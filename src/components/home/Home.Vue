<template>
    <v-app>
        <v-navigation-drawer fixed v-model="drawer" app>
            <v-list dense>
                <v-list-tile @click="">
                    <v-list-tile-action>
                        <v-icon>home</v-icon>
                    </v-list-tile-action>
                    <v-list-tile-content>
                        <v-list-tile-title>Inicio</v-list-tile-title>
                    </v-list-tile-content>
                </v-list-tile>
                <v-list-group prepend-icon="search" value="true" active-class="purple--text">
                    <v-list-tile slot="activator">
                        <v-list-tile-title>Buscar</v-list-tile-title>
                    </v-list-tile>
                    <v-list-group sub-group no-action active-class="purple--text">
                        <v-list-tile slot="activator">
                            <v-list-tile-title>Año</v-list-tile-title>
                        </v-list-tile>
                        <v-list-tile v-for="(age, i) in ages" :key="i" @click="">
                            <v-list-tile-title v-text="age" @click="buscar(age)"></v-list-tile-title>
                        </v-list-tile>
                    </v-list-group>
                    <v-list-group sub-group no-action active-class="purple--text">
                        <v-list-tile slot="activator">
                            <v-list-tile-title>Género</v-list-tile-title>
                        </v-list-tile>
                        <v-list-tile v-for="(genre, i) in genres" :key="i" @click="">
                            <v-list-tile-title v-text="genre" @click="buscar(genre)"></v-list-tile-title>
                        </v-list-tile>
                    </v-list-group>
                </v-list-group>
            </v-list>
        </v-navigation-drawer>
        <v-toolbar color="purple" dark fixed app>
            <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
            <v-toolbar-title>VueCine</v-toolbar-title>
        </v-toolbar>
        <v-content>
            <app-header></app-header>
            <app-body ref="appBody"></app-body>
        </v-content>
        <!-- Footer -->
        <v-footer dark app>
            <v-container grid-list-xs class="text-xs-center">
                <span class="white--text">VueCine &copy; 2018</span>
            </v-container>
        </v-footer>
    </v-app>
</template>

<script>
import Header from './Header.vue';
import Body from './Body.vue';
import axios from 'axios';
export default {
  data: () => ({
    drawer: false,
    ages: ['2016', '2015', '2014', '2013'],
    genres: ['Action', 'Comedy', 'Cartoon', 'Terror'],
    dataSearch: []
  }),
  props: {
    source: String
  },
  methods: {
    buscar(data) {
      this.dataSearch = [];
      axios
        .get('http://www.omdbapi.com/?apikey=c53bd7a7&s='.concat(data))
        .then(response => {
          //console.log(response.data);
          this.dataSearch = response.data.Search;
          this.$refs.appBody.movies = this.dataSearch;
        })
        .catch(error => {
          this.dataSearch = [];
          console.log(error);
        });
    }
  },
  components: {
    appHeader: Header,
    appBody: Body
  }
};
</script>
