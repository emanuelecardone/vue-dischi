<template>
    <section class="loader w-100 h-100 d-flex flex-column justify-content-center align-items-center bg-warning border border-5 border-white">
        <h2 class="mb-5 text-center">Loading</h2>
        <div class="loading_icon w_50p h_50p rounded-circle"></div>
    </section>
</template>

<script>
export default {
    name: 'Loader'
}
</script>

<style lang="scss" scoped>
    .loading_icon{
        border: 5px solid white;
        border-top-color: transparent;
        animation: loading .5s linear infinite;
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