<template>
    <section class="disk w-100 h-100 d-flex flex-column align-items-center p-4">
        <!-- Verranno letti i valori a cui ho eguagliato diskData (proviene da DisksList) -->
        <img :src="diskData.poster" :alt="'Copertina di ' + diskData.title" class="w_175p h_175p">
        <h4 class="text-white text-center text-uppercase mt-3 mb-4">{{diskData.title}}</h4>
        <span class="fs-4 text-center">{{diskData.author}}</span>
        <span class="fs-4 text-center">{{diskData.year}}</span>
    </section>
</template>

<script>
export default {
    name: 'Disk',
    // Specifico che la prop diskData è un oggetto
    props: {
        diskData: Object
    }
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';

    .disk{
        background-color: $primary_color;

        span{
            color: $secondary_color;
        }
    }
</style>