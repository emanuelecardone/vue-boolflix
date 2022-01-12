<template>
    <!-- Main stampa il titolo che informa l'utente che non c'erano risultati se l'api ha caricato e l'array di film e quello di serie tv sono entrambi vuoti,
         stampa il cards wrapper se l'api ha caricato e vi è almeno un elemento tra i 2 array
         stampa il loader se l'utente ha inviato una ricerca ma l'api non ha caricato entrambi gli array -->
    <main class="w-100 d-flex justify-content-center align-items-center">
        <h2 v-if="apiStatusCopy.apiIsReady && userMovies.length === 0 && userSeries.length === 0">We are sorry but there were no results</h2>
        <div v-else-if="apiStatusCopy.apiIsReady" class="cards_wrapper w-100 h-100 d-flex flex-column justify-content-center align-items-center">
            <div v-if="userMovies.length > 0" class="movies_wrapper w-100 d-flex flex-column">
                <h2 class="text-center my-5">Movies:</h2>
                <Card v-for="movie in userMovies" :key="movie.id" class="w-100 border border-2 border-dark p-2" :details="movie" :flags="flagsList" />
            </div>
            <div v-if="userSeries.length > 0" class="series_wrapper w-100 d-flex flex-column">
                <h2 class="text-center my-5">TV Series:</h2>
                <Card v-for="series in userSeries" :key="series.id" class="w-100 border border-2 border-dark p-2" :details="series" :flags="flagsList" />
            </div>
        </div>
        <Loader v-else-if="!apiStatusCopy.apiIsReady && apiStatusCopy.searchIsDone" />
    </main>
</template>

<script>
import Card from './Card.vue';
import Loader from './Loader.vue';

export default {
    name: 'Main',
    components: {
        Card,
        Loader
    },
    props: {
        userMovies: Array,
        userSeries: Array,
        apiStatusCopy: Object,
        flagsList: Array
    }
}
</script>

<style lang="scss" scoped>

    main{
        min-height: calc(100vh - 150px);
    }
</style>