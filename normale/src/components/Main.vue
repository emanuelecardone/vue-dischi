<template>
    <main class="w-100">
        <DisksList />
    </main>
</template>

<script>
import DisksList from './DisksList.vue';

export default {
    name: 'Main',
    components: {
        DisksList
    }
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';

    main{
        background-color: $tertiary_color;
        /* Nel caso continuassi la web-app in futuro devo rimuovere questo vh-100 */
        height: calc(100vh - 100px);
    }
</style>