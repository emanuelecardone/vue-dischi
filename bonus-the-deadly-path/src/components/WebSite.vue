<template>
    <section class="website w-100 h-100 position-relative">
        <AudioBox class="position-absolute" />
        <Header :dayTimeData="dayTime" />
        <Main />
    </section>    
</template>

<script>
import AudioBox from './AudioBox.vue';
import Header from './Header.vue';
import Main from './Main.vue';

export default {
    name: 'WebSite',
    components: {
        AudioBox,
        Header,
        Main
    },
    props: {
        // Oggetto preso da App che conterrà le informazioni sul giorno e ora attuale
        dayTime: Object
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
@import '../style/variables.scss';

    /* Customizzazione audiobox */
    audio{
        left: 50%;
        top: $header_height;
        transform: translateX(-50%);
    }

    @media screen and (min-width: 768px){
        audio{
            left: calc(100% - 10px);
            transform: translateX(-100%);
        }
    }
</style>