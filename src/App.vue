<template>
  <div id="app" class="w-100">
    <Header @sendFilter="getMovies($event)" />
    <Main :userMovies="moviesToSearch" :apiLoaded="apiStatus.apiIsReady" :userSearched="apiStatus.searchIsdone" />
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
      apiStatus: {
        // Variabile per capire se l'api ha caricato
        apiIsReady: false,
        // Variabile per capire se l'utente ha già cercato qualcosa e il server sta rispondendo (per il loader)
        searchIsDone: false
      }
    };
  },
  methods: {
    // Funzione per ricevere tramite emit la stringa cercata dall'utente da Header e passarla come prop a Main
    getMovies: function(userFilter){
      this.apiStatus.searchIsDone = true;

      // La funzione avviene solo se l'utente non clicca senza inserire almeno un carattere
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
        this.apiStatus.searchIsDone = false;
        this.apiStatus.apiIsReady = true;
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
