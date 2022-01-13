<template>
    <!-- La card mostra la copertina se la src non è null, altrimenti una specie di alt con il titolo del film/serie dato in h2
    La sezione info non ha il d-none ma il v-if se si verifica il mouseenter, se invece si verifica il mouseleave si toglie il v-if
    L'overview a volte è una stringa vuota, in quel caso viene stampato uno span con scritto "Not available"
    Anche il cast e genere hanno lo stesso span di overview in quanto non vi è un cast/genere per tutti i film/serie tv ma alcuni ritornano status 404 -->
    <div @mouseenter="handleHover('enter')" @mouseleave="handleHover('leave')" class="element_card position-relative">
        <img v-if="details.poster_path !== null" class="thumbnail" :src="'https://image.tmdb.org/t/p/' + 'w342' + details.poster_path" alt="Copertina">
        <div v-else class="img_alt h-100 d-flex justify-content-center align-items-center">
            <h2 class="mb-0 text-center">{{details.title ? details.title : details.name}}</h2>
        </div>
        <div v-if="userHover" class="element_card_infos w-100 h-100 position-absolute px-3 pt-5 pb-1">
            <ul class="infos_list p-0">
                <li>
                    <strong>Title:</strong> 
                    <span>{{details.title ? details.title : details.name}}</span>
                </li>
                <li>
                    <strong>Original title:</strong>
                    <span>{{details.original_title ? details.original_title : details.original_name}}</span> 
                </li>
                <li class="language_span d-flex align-items-center">
                    <strong>Language:</strong>   
                    <img v-if="flagOrText === 'flag'" class="w_20p" :src="require('../assets/img/' + details.original_language + '.png')" :alt="'Bandiera ' + details.original_language">
                    <span v-else-if="flagOrText === 'text'">{{details.original_language}}</span>
                </li>
                <li>
                    <strong>Rate:</strong> 
                    <RateStar v-for="star,index in stars.tot" :key="index" :class="{'fas': index < stars.full, 'far': index >= stars.full}" />
                </li>
                <li>
                    <strong>Overview:</strong>
                    <p v-if="details.overview.length > 0" class="d-inline">
                        {{details.overview}}
                    </p>
                    <span v-else>Not available</span>
                </li>
                <li>
                    <strong>Cast:</strong>
                    <div v-if="cast.length > 0" class="actors d-inline">
                        <span v-for="actor,index in cast" :key="index">{{actor}},</span>
                    </div>
                    <span v-else>Not available</span>
                </li>
                <li>
                    <strong>Genres:</strong>
                    <div v-if="genres.length > 0" class="genres d-inline">
                        <span v-for="genre,index in genres" :key="index">{{genre}},</span>
                    </div>
                    <span v-else>Not available</span>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
import RateStar from './RateStar.vue';

export default {
    name: 'Card',
    components: {
        RateStar
    },
    props: {
        details: Object,
        cast: Array,
        genres: Array,
        flags: Array
    },
    data: function(){
        return {
            // Variabile per capire se Language deve stampare un testo o un'immagine
            flagOrText: null,
            // Oggetto per definire quante stelle vuote e quante piene stampare per il voto
            stars: {
                tot: 5,
                full: null,
                empty: null
            },
            // Variabile per gestire mouseenter e mouseleave
            userHover: false
        };
    },
    methods: {
        // Funzione per stabilire se verrà stampata la bandiera o la lingua per scritto
        // Lo span language_span stampa un'immagine al suo interno oppure un testo a seconda della variabile flagOrText
        // La variabile è "flag" se l'array delle bandiere include il linguaggio stesso (sono scritte "it" e "fr" come original_language) altrimenti è text    
        defineLanguage: function(){
            this.flagOrText = (this.flags.includes(this.details.original_language)) ? 'flag' : 'text';
        },
        // Funzione per stabilire quante stelle piene e vuote stampare per esprimere il voto
        // Il voto viene diviso per 2, poi arrotondato per eccesso ed ottengo il numero di stelle su 5
        // A questo punto definisco il numero di stelle piene e vuote
        defineRate: function(){
            const fixedRate = Math.ceil(this.details.vote_average / 2);
            this.stars.full = fixedRate;
            this.stars.empty = this.stars.tot - this.stars.full;
        },
        // Funzione per stampare la sezione infos al mouseenter e toglierla al mouseleave
        handleHover: function(enterOrLeave){
            this.userHover = (enterOrLeave === 'enter') ? true : false;
        }
    },
    created: function(){
        // Stabilisco flag o testo e stelle per il voto al created della Card
        this.defineLanguage();
        this.defineRate();
    }
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';

    .element_card{
        display: flex;
        justify-content: center;
        align-items: center;
        border: 3px solid $primary_color;
        color: $primary_color;

        .img_alt{
            width: 342px;
            min-height: 519px;
        }
        &_infos{
            overflow-y: auto;
            z-index: 100;
            background-color: $tertiary_color;

            ul{
                li{
                    margin-bottom: 5px;

                    span,
                    img,
                    i,
                    p{
                        margin-left: 7.5px;
                    }
                }
            }
        }
    }
</style>