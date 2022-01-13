<template>
    <!-- Main stampa il titolo che informa l'utente che non c'erano risultati se l'api ha caricato e l'array di film e quello di serie tv sono entrambi vuoti,
         stampa il cards wrapper se l'api ha caricato e vi è almeno un elemento tra i 2 array
         stampa il loader se l'utente ha inviato una ricerca ma l'api non ha caricato entrambi gli array
         Il flex grow 0 sulle col è per fixare la disposizione dell'ultima riga -->
    <main class="w-100 d-flex justify-content-center align-items-center">
        <h2 v-if="searchStarted && userMovies.length === 0 && userSeries.length === 0">We are sorry but there were no results</h2>
        <div v-else class="cards_wrapper w-100 h-100 d-flex flex-column justify-content-center align-items-center">
            <div v-if="userMovies.length > 0" class="movies_wrapper w-100">
                <div class="container">
                    <h2 class="my-5">Movies:</h2>
                    <div class="row g-5">
                        <div v-for="movie,index in userMovies" :key="movie.id" class="col flex-grow-0">
                            <div class="card_wrapper d-flex">
                                <Card :details="movie" :cast="moviesCast[index]" :genres="moviesGenres[index]" :flags="flagsList" />
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div v-if="userSeries.length > 0" class="series_wrapper w-100">
                <div class="container">
                    <h2 class="my-5">TV Series:</h2>
                    <div class="row g-5">
                        <div v-for="series,index in userSeries" :key="series.id" class="col flex-grow-0">
                            <div class="card_wrapper d-flex">
                                <Card :details="series" :cast="seriesCast[index]" :genres="seriesGenres[index]" :flags="flagsList" />
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <!-- <Loader v-else-if="!apiStatusCopy.apiIsReady && apiStatusCopy.searchIsDone" /> -->
    </main>
</template>

<script>
import Card from './Card.vue';
// import Loader from './Loader.vue';

export default {
    name: 'Main',
    components: {
        Card,
        // Loader
    },
    props: {
        userMovies: Array,
        userSeries: Array,
        moviesCast: Array,
        seriesCast: Array,
        moviesGenres: Array,
        seriesGenres: Array,
        flagsList: Array,
        searchStarted: Boolean
    }
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';

    main{
        min-height: calc(100vh - 150px);
        color: $primary_color;
        background-color: $secondary_color;
    }
</style>