<template>
    <main class="w-100">
        <!-- Test -->
        <Card v-for="movie,index in userMovies" :key="index" :title="movie.title" :originalTitle="movie.original_title" :language="movie.original_language" :rate="movie.vote_average" />
    </main>
</template>

<script>
import Card from './Card.vue';

export default {
    name: 'Main',
    components: {
        Card
    },
    props: {
        userMovies: Array
    },
    data: function(){
        return {
           
        };
    },
    methods: {
        
    }
}
</script>

<style lang="scss" scoped>

    main{
        height: calc(100% - 150px);
    }
</style>