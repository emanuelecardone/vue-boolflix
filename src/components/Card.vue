<template>
    <div class="element_card text-dark">
        <img class="thumbnail" :src="'https://image.tmdb.org/t/p/' + 'w342' + details.poster_path" alt="Copertina">
        <!-- Per distinguere i jason di movies e series viene stampato uno span in base a type contenente uno o l'altro -->
        <span>Title: 
            <span v-if="type === 'movie'">{{details.title}}</span>
            <span v-else-if="type === 'series'">{{details.name}}</span>
        </span>
        <span>Original title: 
            <span v-if="type === 'movie'">{{details.original_title}}</span>
            <span v-else-if="type === 'series'">{{details.original_name}}</span>
        </span>
        <span class="language_span d-flex justify-content-center align-items-center">Language:  
            <img v-if="flagOrText === 'flag'" class="w_20p ms-1" :src="require('../assets/img/' + details.original_language + '.png')" :alt="'Bandiera ' + details.original_language">
            <span v-else-if="flagOrText === 'text'" class="ms-1">{{details.original_language}}</span>
        </span>
        <span>Rate: {{details.vote_average}}</span>
    </div>
</template>

<script>
export default {
    name: 'Card',
    props: {
        details: Object,
        flags: Array,
        type: String
    },
    data: function(){
        return {
            // Variabile per capire se Language deve stampare un testo o un'immagine
            flagOrText: null
        };
    },
    methods: {
        // Funzione per stabilire se verrà stampata la bandiera o la lingua per scritto
        // Lo span language_span stampa un'immagine al suo interno oppure un testo a seconda della variabile flagOrText
        // La variabile è "flag" se l'array delle bandiere include il linguaggio stesso (sono scritte "it" e "fr" come original_language) altrimenti è text    
        defineLanguage: function(){
            this.flagOrText = (this.flags.includes(this.details.original_language)) ? 'flag' : 'text';
        }
    },
    created: function(){
        // Stabilisco flag o testo al created della Card
        this.defineLanguage();
    }
}
</script>

<style lang="scss" scoped>

    .element_card{
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        gap: 1rem;
    }
</style>