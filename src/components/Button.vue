<template>
    <!-- Button è un componente che prende come unica props il messaggio interno -->
    <a href="#" class="btn btn-primary">{{msg}}</a>
</template>

<script>
export default {
    name: 'Button',
    props: {
        msg: String
    }    
}
</script>

<style lang="scss" scoped>

</style>