<template>
    <header class="w-100 h_150p d-flex align-items-center">
        <div class="container">
            <div class="row row-cols-2">
                <div class="col">
                    <div class="logo_wrapper">
                        <h3 class="logo_title text_red text-uppercase mb-0">boolflix</h3>
                    </div>
                </div>
                <div class="col">
                    <div class="search_area_wrapper w-100 d-flex justify-content-end align-items-center">
                        <SearchBar :userFilters="filters" />
                        <SelectBar :arrayToShow="genres.movies" :IDs="genres.moviesIDs" :type="'movies'" @sendFilter="getFilter($event)" />
                        <SelectBar :arrayToShow="genres.series" :IDs="genres.seriesIDs" :type="'series'" @sendFilter="getFilter($event)" />
                        <Button :msg="'Search'" @click.native="showMovies" />
                    </div>
                </div>
            </div>
        </div>  
    </header>
</template>

<script>
import SearchBar from './SearchBar.vue';
import SelectBar from './SelectBar.vue';
import Button from './Button.vue';

export default {
    name: 'Header',
    components: {
        SearchBar,
        SelectBar,
        Button
    },
    props: {
        genres: Object
    },
    data: function(){
        return {
            filters: {
                // Stringa contenente la parola cercata dall'utente
                nameFilter: ''
            }
        };
    },
    methods: {
        // Funzione per il display dei film col nome cercato (emit per App della stringa inserita dall'utente con tolowercase e trim)
        showMovies: function(){
            if(this.filters.nameFilter.length > 0){
                this.$emit('sendFilter', this.filters.nameFilter.toLowerCase().trim());
                // Reset testo nella input 
                this.filters.nameFilter = ''
            }
        },
        // Funzione per inviare con un altro emit l'id del genere selezionato ad App (in arrivo dall'emit di SelectBar)
        getFilter: function(genreID, type){
            this.$emit('sendGenreFilter', genreID, type);
        }
    }
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';

    header{
        background-color: $tertiary_color;

        .search_area_wrapper{
            gap: 1rem;
        }
    }
</style>