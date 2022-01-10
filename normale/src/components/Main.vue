<template>
    <main class="w-100 position-relative">
        <FilterBar class="position-absolute" />
        <DisksList />
    </main>
</template>

<script>
import FilterBar from './FilterBar.vue';
import DisksList from './DisksList.vue';

export default {
    name: 'Main',
    components: {
        FilterBar,
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

        .filter_wrapper{
            top: 5%;
            left: 50%;
            transform: translateX(-50%);
        }
    }
</style>