<template>
    <footer class="w-100 h_100p justify-content-center align-items-center border-top border-2 border-white">
        <h3 class="text-center text-white text-uppercase mb-0">thanks for playing</h3>
    </footer>
</template>

<script>
export default {
    name: 'Footer'
}
</script>

<style lang="scss" scoped>
    footer{
        display: none;
    }
    @media screen and (min-height: 700px){
        footer{
            display: flex;
        }
    }

</style>