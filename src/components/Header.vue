<template>
    <header class="w-100 h_150p d-flex justify-content-center align-items-center bg-dark">
        <SearchBar :userFilters="filters" class="w-25 me-5" />
        <Button :msg="'Search'" @click.native="showMovies" />
    </header>
</template>

<script>
import SearchBar from './SearchBar.vue';
import Button from './Button.vue';

export default {
    name: 'Header',
    components: {
        SearchBar,
        Button
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
        }
    }
}
</script>

<style lang="scss" scoped>

</style>