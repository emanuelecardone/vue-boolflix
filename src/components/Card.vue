<template>
    <div class="element_card text-dark">
        <span>Title: {{title}}</span>
        <span>Original title: {{originalTitle}}</span>
        <span>Language: {{language}}</span>
        <span>Rate: {{rate}}</span>
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

    .element_card{
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        gap: 1rem;
    }
</style>