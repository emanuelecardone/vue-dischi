<template>
    <div class="filter_wrapper">
        <select class="genre_select" v-model="filters.genre" @change="switchFilter">
            <option value="All">All</option>
            <option v-for="genre,index in availableFilters.genre" :key="index" :value="genre">{{genre}}</option>
        </select>
        <select class="artist_select" v-model="filters.artist" @change="switchFilter">
            <option value="All">All</option>
            <option v-for="artist,index in availableFilters.artist" :key="index" :value="artist">{{artist}}</option>
        </select>
    </div>
</template>

<script>
export default {
    name: 'FilterBar',
    props: {
        disksList: Array,
        availableFilters: Object
    },
    data: function(){
        return {
            filters: {
                // Key filtro genere
                genre: 'All',
                // Key filtro artista
                artist: 'All'
            }
        };
    },
    methods: {
        // Funzione per inviare l'index del filtro scelto a Main
        switchFilter: function(){
            this.$emit('sendFilter', this.filters);
            console.log (this.availableFilters)
        }
    }
}
</script>

<style lang="scss" scoped>

    .filter_wrapper{
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 1rem;
    }
</style>