<template>
    <section class="w-100 h-100 d-flex justify-content-center align-items-center">
        <!-- Se la richiesta all'api è completata e ho l'array riempito, stampo la lista.. -->
        <div v-if="disksArray.length === disksAmount" class="diskslist_wrapper">
            <div class="container">
                <div class="row row-cols-5 gx-5 gy-4">
                    <!-- Ciclo le col con il componente Disk all'interno -->
                    <div v-for="(disk, index) in disksArray" :key="index" class="col">
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
    data: function(){
        return {
            // API dei dischi
            disksApi: 'https://flynn.boolean.careers/exercises/api/array/music',
            // Array che riempirò con quello di dischi che mi darà l'api
            disksArray: [],
            // Definisco per comodità il numero di dischi totale
            disksAmount: null
        };
    },
    created: function(){
        // Ricevo l'array dall'api
        axios.get(this.disksApi)
        .then((response) => {
            // Eguaglio disksArray all'array ricevuto dall'api
            this.disksArray = response.data.response;
            // Definisco quindi la quantità di dischi in base alla lunghezza dell'array dell'api
            this.disksAmount = response.data.response.length;
        });
    }
}
</script>

<style lang="scss" scoped>

</style>