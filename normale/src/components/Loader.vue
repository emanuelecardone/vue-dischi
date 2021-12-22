<template>
    <div class="loader d-flex flex-column justify-content-center align-items-center">
        <h2 class="text-white fw-bold mb-5">Loading</h2>
        <div class="loading_icon rounded-circle w_50p h_50p"></div>
    </div>
</template>

<script>
export default {
    name: 'Loader'
}
</script>

<style lang="scss" scoped>

    .loading_icon{
        animation: loading .5s linear infinite;
        border: 2px solid white;
        border-top-color: transparent;
    }

    @keyframes loading{
        0%{
            transform: rotate(0deg);
        }
        25%{
            transform: rotate(90deg);
        }
        50%{
            transform: rotate(180deg);
        }
        75%{
            transform: rotate(270deg);
        }
        100%{
            transform: rotate(360deg);
        }
    }
</style>