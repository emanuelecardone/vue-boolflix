<template>
    <main class="w-100">
        <!-- Test -->
        <Card />
    </main>
</template>

<script>
import axios from 'axios';
import Card from './Card.vue';

export default {
    name: 'Main',
    components: {
        Card
    }
}
</script>

<style lang="scss" scoped>

    main{
        height: calc(100% - 150px);
    }
</style>