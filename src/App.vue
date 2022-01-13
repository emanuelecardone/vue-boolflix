<template>
  <div id="app" class="w-100 d-flex flex-column justify-content-center align-items-center">
    <Header v-if="allGenresLoaded" @sendFilter="getMoviesAndSeries($event)" @sendGenreFilter="getGenreFilter($event)" :genres="allGenres" />
    <Main v-if="allGenresLoaded" :userMovies="moviesToSearch" :userSeries="seriesToSearch" :flagsList="flags" :searchStarted="searchOn" :searchEnded="searchOff" />
    <Loader v-else />
  </div>
</template>

<script>
import axios from 'axios';
import Header from './components/Header.vue';
import Main from './components/Main.vue';
import Loader from './components/Loader.vue';

export default {
  name: "App",
  components: {
    Header,
    Main,
    Loader
  },
  data: function() {
    return {
      // Variabile per capire se l'utente ha già cercato qualcosa ma la risposta non è ancora arrivata
      searchOn: false,
      // Variabile come prima ma per capire se l'api ha caricato
      searchOff: false,
      // Variabile per capire se i film sono stati caricati
      moviesLoaded: false,
      // Variabile per capire se le serie tv sono state caricate
      seriesLoaded: false,
      // Variabile per capire se i 2 array con tutti i generi possibili sono stati caricati
      allGenresLoaded: false,
      // Oggetto contenente tutti i generi possibili per film e movie (sono 2 chiamate separate)
      allGenres: {
        movies: [],
        moviesIDs: [],
        series: [],
        seriesIDs: []
      },
      // Array vuoto di default da riempire con l'array dei film dopo la ricerca dell'utente
      moviesToSearch: [],
      // Array vuoto di default da riempire con l'array delle serie tv dopo la ricerca dell'utente
      seriesToSearch: [],
      // Variabile con la key dell'api
      apiKey: 'be363ff2ab5080629cc952123e4f9fd8',
      // Array contenente le bandiere
      flags: ['it', 'fr'],
      // Oggetto contenente i generi scelti dall'utente per filtrare (hanno 'All' di default)
      filters: {
        movies: 'All',
        series: 'All'
      }
    };
  },
  methods: {
    // Funzione per ricevere tramite emit la stringa cercata dall'utente da Header e passarla come prop a Main
    getMoviesAndSeries: function(userFilter){

      // Variabile per far capire a Main che l'utente ha cercato
      this.searchOn = true;
      // Variabile per capire che la ricerca non ha ancora caricato l'api
      this.searchOff = false;
      // Movies e Series hanno false su loaded ad ogni inizio chiamata
      this.moviesLoaded = false;
      this.seriesLoaded = false;

      // Chiamata api per i film
      axios.get(
        'https://api.themoviedb.org/3/search/movie',
        {
          params: {
            api_key: this.apiKey,
            // La query prende il valore di nameFilter modellato dalla input
            query: userFilter
          }
        }
      )
      .then((response) => {
        // Salvo l'array ritornato dall'api in moviesToSearch
        this.moviesToSearch = response.data.results;
        // Filtro l'array
        this.moviesToSearch = this.filteringMovies;
        // Il load di movies diventa true e, se anche quello di series è true, allora la ricerca completa è finita
        this.moviesLoaded = true;
        if(this.seriesLoaded){
          this.searchOn = false;
          this.searchOff = true;
        }
      });

      // Chiamata api per le serie tv
      axios.get(
        'https://api.themoviedb.org/3/search/tv',
        {
          params: {
            api_key: this.apiKey,
            // La query prende il valore di nameFilter modellato dalla input
            query: userFilter
          }
        }
      )
      .then((response) => {
        // Salvo l'array ritornato dall'api in seriesToSearch
        this.seriesToSearch = response.data.results;
        // Filtro l'array
        this.seriesToSearch = this.filteringSeries;
        // Il load di series diventa true e, se anche quello di movies è true, allora la ricerca completa è finita
        this.seriesLoaded = true;
        if(this.moviesLoaded){
          this.searchOn = false;
          this.searchOff = true;
        }
      });
    },
    
    // Funzione per ricevere l'array di generi possibili per film e serie tv (sono 2 chiamate separate)
    getAllGenres: function(){
      // Generi per i film
      axios.get(
        'https://api.themoviedb.org/3/genre/movie/list',
        {
          params: {
            api_key: this.apiKey
          }
        }
      )
      .then((response) => {
        // forEach sulla risposta in modo da pushare nell'array allGenres.movies solo il nome del genere
        response.data.genres.forEach((genre) => {
          this.allGenres.movies.push(genre.name);
          this.allGenres.moviesIDs.push(genre.id);
        });
        // Variabile loader
        if(this.allGenres.series.length > 0){
          this.allGenresLoaded = true;
        }
      });

      // Generi per le serie
      axios.get(
        'https://api.themoviedb.org/3/genre/tv/list',
        {
          params: {
            api_key: this.apiKey
          }
        }
      )
      .then((response) => {
        // forEach sulla risposta in modo da pushare nell'array allGenres.series solo il nome del genere
        response.data.genres.forEach((genre) => {
          this.allGenres.series.push(genre.name);
          this.allGenres.seriesIDs.push(genre.id);
        });
        // Variabile loader
        if(this.allGenres.movies.length > 0){
          this.allGenresLoaded = true;
        }
      });
    },
    // Funzione per salvare in una variabile il genere selezionato
    getGenreFilter: function(filterObject){
      filterObject.arrayType === 'movies' ? this.filters.movies = filterObject.id : this.filters.series = filterObject.id;
    }
  
  },
  computed: {
    // Funzione per filtrare l'array di film dopo la ricerca in base al genere selezionato
    filteringMovies: function(){
      if(this.filters.movies === 'All'){
          return this.moviesToSearch;
      }
      const filteredArray = this.moviesToSearch.filter((movie) => {
        return movie.genre_ids.includes(this.filters.movies);
      });
      
      return filteredArray;
    },
    // Funzione per filtrare l'array di serie tv dopo la ricerca in base al genere selezionato
    filteringSeries: function(){
      if(this.filters.series === 'All'){
        return this.seriesToSearch;
      }
      const filteredArray = this.seriesToSearch.filter((series) => {
        return series.genre_ids.includes(this.filters.series);
      });
    
      return filteredArray;
    }
  },
  created: function(){
    // Richiamo la funzione per avere gli array con tutti i generi possibili per film e serie tv
    this.getAllGenres();
  }
};
</script>

<style lang="scss">
@import './style/variables.scss';
@import './style/mixins.scss';
@import './style/general.scss';
@import './style/size-space.scss';
@import './style/utilities.scss';

  /* Per far vedere il Loader al centro della pagina */
  #app{
    min-height: 100vh;
  }
</style>
