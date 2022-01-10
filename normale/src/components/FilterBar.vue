<template>
    <div class="filter_wrapper">
        <select class="genre_select" v-model="filters.genre" @change="switchFilter">
            <option value="All">All</option>
            <option value="Rock">Rock</option>
            <option value="Pop">Pop</option>
            <option value="Jazz">Jazz</option>
            <option value="Jazz">Jazz</option>
        </select>
        <select class="artist_select" v-model="filters.artist" @change="switchFilter">
            <option value="All">All</option>
            <option value="Bon Jovi">Bon Jovi</option>
            <option value="Queen">Queen</option>
            <option value="Sting">Sting</option>
            <option value="Steve Gadd Band">Steve Gadd Band</option>
            <option value="Iron Maiden">Iron Maiden</option>
            <option value="Eric Clapton">Eric Clapton</option>
            <option value="Deep Purple">Deep Purple</option>
            <option value="Metallica">Metallica</option>
            <option value="Dave Weckl">Dave Weckl</option>
            <option value="Michael Jacjson">Michael Jackson</option>
        </select>
    </div>
</template>

<script>
export default {
    name: 'FilterBar',
    props: {
        disksList: Array
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
            console.log (this.filters)
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