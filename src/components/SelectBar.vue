<template>
    <!-- La SelectBar viene richiamata sia per mostrare tutti i generi di film che di serie tv, ha la prima opzione All per entrambe
    Il value di ogni option (a parte All) è l'id del genere, in modo da consentire il filtro con genre_ids dell'api -->
    <select v-model="genreID" @change="switchFilter">
        <option value="All">{{type === 'movies' ? 'All Movies' : 'All Series' }}</option>
        <option v-for="genre,index in arrayToShow" :key="index" :value="IDs[index]">{{genre}}</option>
    </select>
</template>

<script>
export default {
    name: 'SelectBar',
    props: {
        arrayToShow: Array,
        IDs: Array,
        type: String
    },
    data: function(){
        return {
            // Variabile che viene modellata dal change della select ed inquadra l'id del genere
            genreID: 'All'
        };
    },
    methods: {
        // Funzione per inviare tramite emit il filtro selezionato dall'utente ad Header e il tipo
        switchFilter: function(){
            this.$emit(
                'sendFilter', 
                {
                id: this.genreID,
                arrayType: this.type
                } 
            );
        }
    }    
}
</script>

<style lang="scss" scoped>

</style>