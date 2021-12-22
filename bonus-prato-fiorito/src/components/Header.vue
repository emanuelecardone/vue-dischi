<template>
    <header class="w-100 h_100p justify_content_evenly align-items-center border-bottom border-2 border-white text-center text-uppercase text-white">
        <h2 class="mb-0">prato fiorito</h2>
        <img class="w_75p" :src="imageSrc" alt="Logo di Prato Fiorito">
    </header>
</template>

<script>
export default {
    name: 'Header',
    data: function(){
        return {
            imageSrc: require('../assets/img/prato-fiorito-logo.png')
        };
    }
}
</script>

<style lang="scss" scoped>
    header{
        display: none;
    }
    @media screen and (min-height: 700px){
        header{
            display: flex;
        }
    }
</style>