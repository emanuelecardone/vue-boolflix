<template>
    <!-- RateStar è la stella che rappresenta il voto del film/serie, ha classe "fas" per la stella piena e classe "far" per la stella vuota, seguiti da "fa-star" di default -->
    <i :class="' fa-star'"></i>
</template>

<script>
export default {
    name: 'RateStar'
}
</script>

<style lang="scss" scoped>

    [class*="fa-star"]{
        color: goldenrod;
    }
</style>