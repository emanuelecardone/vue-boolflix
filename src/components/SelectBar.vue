<template>
    <select>
        <option>{{type === 'movies' ? 'All movies' : 'All Series' }}</option>
        <option v-for="genre,index in arrayToShow" :key="index" :value="genre">{{genre}}</option>
    </select>
</template>

<script>
export default {
    name: 'SelectBar',
    props: {
        arrayToShow: Array,
        type: String
    }    
}
</script>

<style lang="scss" scoped>

</style>