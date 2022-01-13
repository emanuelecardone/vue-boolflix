<template>
  <div id="app" class="w-100">
    <Header @sendFilter="getMoviesAndSeries($event)" />
    <Main :userMovies="moviesToSearch" :userSeries="seriesToSearch" :moviesCast="cast.movies" :seriesCast="cast.series" :moviesGenres="genres.movies" :seriesGenres="genres.series" :flagsList="flags" :searchStarted="userSearched" />
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
      // Variabile per capire se l'utente ha già cercato qualcosa
      userSearched: false,
      // Array vuoto di default da riempire con l'array dei film dopo la ricerca dell'utente
      moviesToSearch: [],
      // Array vuoto di default da riempire con l'array delle serie tv dopo la ricerca dell'utente
      seriesToSearch: [],
      // Oggetto contenente i cast per movies e series
      cast: {
        movies: [],
        series: []
      },
      // Oggetto contenente i generi per movies e series
      genres: {
        movies: [],
        series: []
      },
      // Variabile con la key dell'api
      apiKey: 'be363ff2ab5080629cc952123e4f9fd8',
      // Array contenente le bandiere
      flags: ['it', 'fr']
    };
  },
  methods: {
    // Funzione per ricevere tramite emit la stringa cercata dall'utente da Header e passarla come prop a Main
    getMoviesAndSeries: function(userFilter){

      // Variabile per far capire a Main che l'utente ha cercato
      this.userSearched = true;

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
        // Richiamo la funzione per ricevere il cast del film dall'api
        this.getCast(this.moviesToSearch, 'movie');
        // Richiamo la funzione per ricevere i generi del film dall'api
        this.getGenres(this.moviesToSearch, 'movie');
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
        // Richiamo la funzione per ricevere il cast della serie dall'api
        this.getCast(this.seriesToSearch, 'series');
        // Richiamo la funzione per ricevere i generi della serie dall'api
        this.getGenres(this.seriesToSearch, 'series');
      });
    },
    // Funzione per ricevere il cast del film/serie con un max di 5 attori
    getCast: function(arrayToCheck, type){
      arrayToCheck.forEach((movieOrSeries) => {
        // Creo un array vuoto che riempirò con un max di 5 attori, poi lo pusho in cast.movies o cast.series che saranno quindi matrici
        const thisCast = [];
        // Chiamata api per ricevere il cast del film/serie corrente
        axios.get(
          'https://api.themoviedb.org/3/movie/' + movieOrSeries.id + '/credits',
          {
            params: {
              api_key: this.apiKey
            }
          }
        )
        .then((response) => {
          // La risposta dell'api è un array in cui ogni oggetto contiene le info sull'attore, a me serve solo name
          // Ho bisogno quindi di fare un altro forEach sulla risposta dell'api, così da pushare name in thisCast
          response.data.cast.forEach((actor,index) => {
            if(index < 5 && response.status === 200){
              thisCast.push(actor.name);
            }
          });
          // Adesso posso pushare thisCast nell'array principale, type mi aiuta a capire dove pushare
          type === 'movie' ? this.cast.movies.push(thisCast) : this.cast.series.push(thisCast);
        });
      });
    },
    // Funzione per ricevere i generi del film/serie con un max di 5 attori
    getGenres: function(arrayToCheck, type){
      arrayToCheck.forEach((movieOrSeries) => {
        // Creo un array vuoto che riempirò con i generi relativi, poi lo pusho in genres.movies o genres.series che saranno quindi matrici
        const theseGenres = [];
        // Chiamata api per ricevere i generi del film/serie corrente
        axios.get(
          'https://api.themoviedb.org/3/movie/' + movieOrSeries.id,
          {
            params: {
              api_key: this.apiKey
            }
          }
        )
        .then((response) => {
          // La risposta dell'api è un array in cui ogni oggetto contiene le info principali del film/serie
          // Ho bisogno quindi di fare un altro forEach sulla risposta dell'api, così da pushare genres.name in thisCast
          response.data.genres.forEach((genre) => {
            if(response.status === 200){
              theseGenres.push(genre.name);
            }
          });
          // Adesso posso theseGenres thisCast nell'array principale, type mi aiuta a capire dove pushare
          type === 'movie' ? this.genres.movies.push(theseGenres) : this.genres.series.push(theseGenres);
        });
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
