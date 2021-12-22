<template>
    <section class="single_card_wrapper w-100 h-100 d-flex justify-content-center align-items-center">
        <div class="card w-75 h-75 p-5 bg-warning border border-5 border-white flex-row justify_content_around">
            <img :src="characterInfos.image" :alt="'Immagine di ' + characterInfos.name">
            <div class="card-body d-flex flex-column justify-content-between align-items-center text-white text-center">
                <h2 class="mb-0 fs-1">{{characterInfos.name}}</h2>
                <ul class="p-0">
                    <template v-for="(value, info, index) in characterInfos">
                        <li v-if="info !== 'name' && info !== 'id' && info !== 'image'" :key="index">
                            <span class="text-capitalize fs-4">{{info}}: {{value}}</span>
                        </li>
                    </template>
                </ul>
            </div>
        </div>
    </section>
</template>

<script>
export default {
    name: 'Card',
    props: {
        characterInfos: Object
    }
}
</script>

<style lang="scss" scoped>

</style>