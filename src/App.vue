<template>
  <div id="app" class="w-100 vh-100 bg-dark">
    <Header @sendMovies="getMovies($event)" />
    <Main :userMovies="moviesToSearch" />
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
      moviesToSearch: null
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
