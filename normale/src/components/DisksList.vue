<template>
    <section class="diskslist_wrapper">
        <div class="container">
            <div class="row row-cols-5">
                <div class="col">
                    <Disk />
                </div>
            </div>
        </div>
    </section>
</template>

<script>
import axios from 'axios';
import Disk from './Disk.vue';

export default {
    name: 'DisksList',
    components: {
        Disk
    },
    data: function(){
        return {
            disksApi: 'https://flynn.boolean.careers/exercises/api/array/music',
            disksArray: []
        };
    },
    created: function(){
        axios.get(this.disksApi)
        .then((response) => {
            this.disksArray = response.data.response;
        });
    }
}
</script>

<style lang="scss" scoped>

</style>