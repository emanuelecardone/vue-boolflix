<template>
    <div class="element_card text-white">
        Title: {{title}}
        Original title: {{originalTitle}}
        Language: {{language}}
        Rate: {{rate}}
    </div>
</template>

<script>
export default {
    name: 'Card',
    props: {
        title: String,
        originalTitle: String,
        language: String,
        rate: Number
    }
}
</script>

<style lang="scss" scoped>

</style>