<template>
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