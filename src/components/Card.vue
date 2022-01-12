<template>
    <div class="element_card text-dark">
        <span>Title: {{details.title}}</span>
        <span>Original title: {{details.original_title}}</span>
        <span>Language: {{details.original_language}}</span>
        <span>Rate: {{details.vote_average}}</span>
    </div>
</template>

<script>
export default {
    name: 'Card',
    props: {
        details: Object
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