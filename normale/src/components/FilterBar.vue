<template>
    <div class="filter_wrapper">
        <select class="genre_select" v-model="genreFilter">
            <option value="x">test1</option>
            <option value="y">test2</option>
            <option value="z">test3</option>
        </select>
        <select class="artist_select" v-model="artistFilter">
            <option value="x">test1</option>
            <option value="y">test2</option>
            <option value="z">test3</option>
        </select>
    </div>
</template>

<script>
export default {
    name: 'FilterBar',
    data: function(){
        return {
            // Variabile filtro genere
            genreFilter: null,
            // Variabile filtro artista
            artistFilter: null
        };
    }    
}
</script>

<style lang="scss" scoped>

</style>