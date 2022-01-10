<template>
    <section class="welcome">
        <h1 class="fs_50 fw-bold text-uppercase text-center">welcome</h1>
        <div class="icons_wrapper w-100 d-flex justify-content-between align-items-center">
            <img class="icon" :src="iconSrc" alt="Logo del gioco">
            <img class="big_icon" :src="bigIconSrc" alt="Logo grande del gioco">
            <img class="icon" :src="iconSrc" alt="Logo del gioco">
        </div>
        <Button @click.native="leaveSection" :msg="'start'" />
    </section>
</template>

<script>
import Button from './Button.vue';

export default {
    name: 'Welcome',
    components: {
        Button
    },
    props:{
        status: Object
    },
    data: function(){
        return {
            bigIconSrc: require('../assets/img/logo_small.png'),
            iconSrc: require('../assets/img/logo_small_icon_only.png')
        };
    },
    methods: {
        // Funzione per passare alla sezione successiva
        leaveSection: function(){
            this.status.welcome = false;
            this.status.tutorial = true;
        }
    }
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';

    .welcome{
        
        .big_icon{
            border-radius: 20px;
            width: 25%;
        }
        .icon{
            width: 25%;
        }
    }

</style>