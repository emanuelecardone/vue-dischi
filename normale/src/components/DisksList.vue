<template>
    <section class="w-100 h-100 d-flex justify-content-center align-items-center">
        <!-- Se la richiesta all'api è completata e ho l'array riempito, stampo la lista.. -->
        <div v-if="disksArray.length === disksAmount" class="diskslist_wrapper w-100">
            <div class="container">
                <div class="row row-cols-5 gx-5 gy-4">
                    <!-- Ciclo le col con il componente Disk all'interno -->
                    <div v-for="(disk, index) in displayDisks" :key="index" class="col">
                        <!-- Faccio capire al programma che la prop diskData è il disco corrente, ovvero l'oggetto corrente -->
                        <Disk :diskData="disk"/>
                    </div>
                </div>
            </div>
        </div>
        <!-- ..altrimenti stampo il loader -->
        <Loader v-else />
        
    </section>
</template>

<script>
import axios from 'axios';
import Disk from './Disk.vue';
import Loader from './Loader.vue';

export default {
    name: 'DisksList',
    components: {
        Disk,
        Loader
    },
    props: {
        filters: Object
    },
    data: function(){
        return {
            // API dei dischi
            disksApi: 'https://flynn.boolean.careers/exercises/api/array/music',
            // Array che riempirò con quello di dischi che mi darà l'api
            disksArray: [],
            // Definisco per comodità il numero di dischi totale
            disksAmount: null,
            // Oggetto con i 2 array per genere e artista
            filterArrays: {
                genre: [],
                artist: []
            }
        };
    },
    methods: {
        // Funzione per creare gli array di generi e artisti
        defineFilters: function(){
            this.disksArray.forEach((disk) => {
                !this.filterArrays.genre.includes(disk.genre) ? this.filterArrays.genre.push(disk.genre) : null;
                !this.filterArrays.artist.includes(disk.author) ? this.filterArrays.artist.push(disk.author) : null;
            });

            // Emit per passare i due array a Main
            this.$emit('sendPossibleFilters', this.filterArrays);
        }
    },
    computed: {
        // Funzione che ritorna l'array da dare in display, in base al filtro o meno
        displayDisks: function(){
            // Se entrambi i filtri sono All, allora ritorna l'array originale
            if(this.filters.genre === 'All' && this.filters.artist === 'All'){
                return this.disksArray;
            }

            // Procedo a filtrare se i filtri sono diversi da All
            const filteredDisks = this.disksArray.filter((disk) => {
                if(this.filters.artist === 'All'){
                    // Filtro per genere se artist è All
                    return disk.genre === this.filters.genre;
                } else if (this.filters.genre === 'All'){
                    // Filtro per artist se genere è All
                    return disk.author === this.filters.artist;
                } else {
                    // Filtro per entrambi
                    return disk.genre === this.filters.genre && disk.author === this.filters.artist;
                }
            });
            // Ritorno array filtrato
            return filteredDisks;
        }
    },
    created: function(){
        // Ricevo l'array dall'api
        axios.get(this.disksApi)
        .then((response) => {
            // Eguaglio disksArray all'array ricevuto dall'api
            this.disksArray = response.data.response;
            // Definisco quindi la quantità di dischi in base alla lunghezza dell'array dell'api
            this.disksAmount = response.data.response.length;
            // Richiamo la funzione per creare gli array con genere e artista
            this.defineFilters();
        });
    }
}
</script>

<style lang="scss" scoped>

</style>