<template>
    <!-- Button di bootstrap, verrà usato in tutte le sezioni quindi lo uso come componente -->
    <button class="game_button btn btn-outline-success" type="button">{{msg}}</button>
</template>

<script>
export default {
    name: 'Button',
    props: {
        msg: String
    }
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';
@import '../style/mixins.scss';

    .game_button{
        @include game_button;

        &:hover{
            background-color: lighten($color: $secondary_color, $amount: 10%);
            border: 3px solid $secondary_color;
            color: $tertiary_color;
            transform: scale(1.2);
        }
    }
</style>
