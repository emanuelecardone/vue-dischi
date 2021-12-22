<template>
    <header class="w-100 h_100p d-flex align-items-center px-4">
        <img :src="logoSrc" alt="Logo di Spotify" class="w_50p">
    </header>
</template>

<script>
export default {
    name: 'Header',
    data: function(){
        return {
            logoSrc: require('../assets/img/spotify-logo.png')
        };
    }
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';

    header{
        background-color: $primary_color;
    }

</style>