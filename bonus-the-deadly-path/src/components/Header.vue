<template>
    <header class="w-100 h_150p fixed-top">
        <UpperHeader class="w-100" :dayTime="dayTimeData" />
        <Decoration class="position-absolute" />
        <LowerHeader class="w-100" />
    </header>
</template>

<script>
import UpperHeader from './UpperHeader.vue';
import Decoration from './Decoration.vue';
import LowerHeader from './LowerHeader.vue';

export default {
    name: 'Header',
    components: {
        UpperHeader,
        Decoration,
        LowerHeader
    },
    props:{
        dayTimeData: Object
    },
    data: function(){
        return {

        };
    },
    methods: {
        
    }
}
</script>

<style lang="scss" scoped>
    
</style>