<template>
    <section class="slider_wrapper w-75 h-75">
        <div v-if="characters.length === charactersAmount" class="slider w-100 h-100 d-flex justify-content-center align-items-center">
            <i @click="changeActiveItem('left')" class="fas fa-arrow-left bg-warning p-3 rounded-circle border border-4 border-white"></i>
            <div class="cards_wrapper w-100 h-100">
                <Card :characterInfos="characters[currentCharacter]" />
            </div>
            <i @click="changeActiveItem('right')" class="fas fa-arrow-right bg-warning p-3 rounded-circle border border-4 border-white"></i>
        </div>
        <Loader v-else />
    </section>
</template>

<script>
import axios from 'axios';
import Card from './Card.vue';
import Loader from './Loader.vue';

export default {
    name: 'Slider',
    components: {
        Card,
        Loader
    },
    data: function(){
        return {
            apiSrc: 'https://api.sampleapis.com/rickandmorty/characters',
            characters: [],
            charactersAmount: null,
            currentCharacter: 0
        };
    },
    methods: {
        // Funzione unica che cambia l'active item in base alla freccia clickata
        changeActiveItem: function(thisDirection){
            if(thisDirection === 'left'){
                this.currentCharacter = (this.currentCharacter > 0) ? this.currentCharacter - 1 : this.characters.length - 1;
                
            } else{
                this.currentCharacter = (this.currentCharacter < this.characters.length - 1) ? this.currentCharacter + 1 : 0;
            }
        }
    },
    // Ricezione risposta api al created
    created: function(){
        axios.get(this.apiSrc)
        .then((response) => {
            this.characters = response.data;
            this.charactersAmount = response.data.length;
        });
    }
}
</script>

<style lang="scss" scoped>
    
</style>