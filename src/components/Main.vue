<template>
    <!-- Main stampa il titolo che informa l'utente che non c'erano risultati se l'api ha caricato e l'array è vuoto,
         stampa il cards wrapper se l'api ha caricato e vi è almeno un elemento
         stampa il loader se l'utente ha inviato una ricerca ma l'api non ha caricato -->
    <main class="w-100 d-flex justify-content-center align-items-center">
        <h2 v-if="apiLoaded && userMovies.length === 0">We are sorry but there were no results</h2>
        <div v-else-if="apiLoaded" class="cards_wrapper w-100 h-100 d-flex flex-wrap">
            <Card v-for="movie in userMovies" :key="movie.id" class="w-50 border border-2 border-dark p-2" :details="movie" />
        </div>
        <Loader v-else-if="!apiLoaded && userSearched" />
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
        apiLoaded: Boolean,
        userSearched: Boolean
    }
}
</script>

<style lang="scss" scoped>

    main{
        min-height: calc(100vh - 150px);
    }
</style>