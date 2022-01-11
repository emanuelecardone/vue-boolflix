<template>
    <header class="w-100 h_150p d-flex justify-content-center align-items-center bg-dark">
        <SearchBar :userFilters="filters" class="w-25 me-5" />
        <Button :msg="'Search'" @click.native="showMovies" />
    </header>
</template>

<script>
import axios from 'axios';
import SearchBar from './SearchBar.vue';
import Button from './Button.vue';

export default {
    name: 'Header',
    components: {
        SearchBar,
        Button
    },
    props: {
        apiUtilities: Object
    },
    data: function(){
        return {
            filters: {
                // Stringa contenente la parola cercata dall'utente
                nameFilter: null
            }
        };
    },
    methods: {
        // Funzione per il display dei film col nome cercato
        showMovies: function(){
            this.apiUtilities.searchIsDone = true;

            // La funzione avviene solo se l'utente non clicca senza inserire almeno un carattere
            if(this.filters.nameFilter !== null){
                axios.get(
                    'https://api.themoviedb.org/3/search/movie',
                    {
                        params: {
                            api_key: 'be363ff2ab5080629cc952123e4f9fd8',
                            // La query prende il valore di nameFilter modellato dalla input
                            query: this.filters.nameFilter
                        }
                    }
                )
                .then((response) => {
                    // Emit per mandare ad App l'array di film usciti come risultato
                    this.$emit('sendMovies', response.data.results);
                    this.apiUtilities.searchIsDone = false;
                    this.apiUtilities.apiIsReady = true;
                });
            }
        }
    }
}
</script>

<style lang="scss" scoped>

</style>