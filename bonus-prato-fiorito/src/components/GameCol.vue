<template>
    <section class="game_col d-flex justify-content-center align-items-center text-white" :class="colSize">
        <span>{{colNumber}}</span>
    </section>
</template>

<script>
export default {
    name: 'GameCol',
    props: ['colNumber', 'colSize']

}
</script>

<style lang="scss" scoped>
    
</style>