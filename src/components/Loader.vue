<template>
    <div class="loader w-100 h-100 d-flex flex-column justify-content-center align-items-center">
        <h3>Loading</h3>
        <div class="loading_circle w_30p h_30p"></div>
    </div>
</template>

<script>
export default {
    name: 'Loader'
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';

    .loader{
        color: $primary_color;
    }

    .loading_circle{
        border: 3px solid $primary_color;
        border-top-color: transparent;
        border-radius: 50%;
        animation: loading .5s linear infinite;
    }

    @keyframes loading{
        0%{
            transform: rotate(0deg);
        }
        100%{
            transform: rotate(360deg);
        }
    }
</style>