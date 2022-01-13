<template>
    <!-- La SearchBar inquadra il filtro tramite il nome o parte del nome inserito dall'utente -->
    <input type="search" v-model="userFilters.nameFilter" class="form-control form-control-md" placeholder="Type the name here">
</template>

<script>
export default {
    name: 'SearchBar',
    props: {
        userFilters: Object
    }    
}
</script>

<style lang="scss" scoped>

</style>