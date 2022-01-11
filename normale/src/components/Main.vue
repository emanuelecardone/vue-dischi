<template>
    <main class="w-100 position-relative">
        <FilterBar class="position-absolute" :disksList="disks" :availableFilters="possibleFilters" @sendFilters="getFilters($event)" />
        <DisksList :filters="userFilter"  @sendPossibleFilters="getPossibleFilters($event)" />
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
    },
    data: function(){
        return {
            // Variabile nella quale salverò l'array di dischi in arrivo da DisksList
            disks: null,
            // Variabile nella quale salverò l'oggetto con gli indici dei filtri
            userFilter: {
                genre: 'All',
                artist: 'All'
            },
            // Oggetto che verrà riempito con tutti i generi e artisti possibili e verrà passato a FilterBar come props
            possibleFilters: {}
        };
    },
    methods: {
        // Funzione per ricevere gli index dei filtri da FilterBar
        getFilter: function(filters){
            this.userFilter = filters;
        },
        // Funzione per ricevere i generi e artisti possibili
        getPossibleFilters: function(possibleFiltersObject){
            this.possibleFilters = possibleFiltersObject;
            console.log(this.po)
        }
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