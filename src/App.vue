<template>
  <div id="app" class="w-100">
    <Header @sendMovies="getMovies($event)" :apiUtilities="apiStatus" />
    <Main :userMovies="moviesToSearch" :apiLoaded="apiStatus.apiIsReady" :userSearched="apiStatus.searchIsdone" />
  </div>
</template>

<script>
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
    getMovies: function(filteredMovies){
      this.moviesToSearch = filteredMovies;
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
