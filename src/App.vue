<template>
  <div id="app" class="w-100">
    <Header @sendFilter="getMoviesAndSeries($event)" />
    <Main :userMovies="moviesToSearch" :userSeries="seriesToSearch" :apiStatusCopy="apiStatus" :flagsList="flags" />
  </div>
</template>

<script>
import axios from 'axios';
import Header from './components/Header.vue';
import Main from './components/Main.vue';

export default {
  name: "App",
  components: {
    Header,
    Main
  },
  data: function() {
    return {
      // Variabile vuota di default da riempire con l'array dei film dopo la ricerca dell'utente
      moviesToSearch: [],
      // Variabile vuota di default da riempire con l'array delle serie tv dopo la ricerca dell'utente
      seriesToSearch: [],
      apiStatus: {
        // Variabile per capire se Movies ha caricato
        moviesAreReady: null,
         // Variabile per capire se Series ha caricato
        seriesAreReady: null,
        // Variabile per capire se l'api intero ha caricato
        apiIsReady: false,
        // Variabile per capire se l'utente ha già cercato qualcosa e il server sta rispondendo (per il loader)
        searchIsDone: false
      },
      // Array contenente le bandiere
      flags: ['it', 'fr']
    };
  },
  methods: {
    // Funzione per ricevere tramite emit la stringa cercata dall'utente da Header e passarla come prop a Main
    getMoviesAndSeries: function(userFilter){
      this.apiStatus.searchIsDone = true;
      this.moviesAreReady = false;
      this.seriesAreReady = false;

      // Chiamata api per i film
      axios.get(
        'https://api.themoviedb.org/3/search/movie',
        {
          params: {
            api_key: 'be363ff2ab5080629cc952123e4f9fd8',
            // La query prende il valore di nameFilter modellato dalla input
            query: userFilter
          }
        }
      )
      .then((response) => {
        // Salvo l'array ritornato dall'api in moviesToSearch
        this.moviesToSearch = response.data.results;
        this.moviesAreReady = true;
        if(this.seriesAreReady){
          this.apiStatus.searchIsDone = false;
          this.apiStatus.apiIsReady = true;
        }
      });

      // Chiamata api per le serie tv
      axios.get(
        'https://api.themoviedb.org/3/search/tv',
        {
          params: {
            api_key: 'be363ff2ab5080629cc952123e4f9fd8',
            // La query prende il valore di nameFilter modellato dalla input
            query: userFilter
          }
        }
      )
      .then((response) => {
        // Salvo l'array ritornato dall'api in seriesToSearch
        this.seriesToSearch = response.data.results;
        this.seriesAreReady = true;
        if(this.moviesAreReady){
          this.apiStatus.searchIsDone = false;
          this.apiStatus.apiIsReady = true;
        }
      });
    }
  }
};
</script>

<style lang="scss">
@import './style/variables.scss';
@import './style/mixins.scss';
@import './style/general.scss';
@import './style/size-space.scss';
@import './style/utilities.scss';

</style>
