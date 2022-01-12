<template>
    <i :class="starClass + ' fa-star'"></i>
</template>

<script>
export default {
    name: 'RateStar',
    props: {
        starClass: String
    }    
}
</script>

<style lang="scss" scoped>

    [class*="fa-star"]{
        color: goldenrod;
    }
</style>