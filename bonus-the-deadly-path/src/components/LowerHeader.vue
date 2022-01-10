<template>
    <section class="lower_header w-100">
        <NavBar />
    </section>
</template>

<script>
// Importo il componente NavBar
import NavBar from './NavBar.vue';

export default {
    name: 'LowerHeader',
    components: {
        NavBar
    },
    methods: {
        
    }
}
</script>

<style lang="scss" scoped>
    /* Caratteristiche per lower header */
    .lower_header{
        height: calc(100% / 3 * 2);
    }
</style>