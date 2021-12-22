<template>
    <main class="w-100 d-flex justify-content-center align-items-center text-white fs-1">
        <Slider />
    </main>
</template>

<script>
import Slider from './Slider.vue';

export default {
    name: 'Main',
    components: {
        Slider
    }
}
</script>

<style lang="scss" scoped>
    main{
        height: calc(100% - 200px);
    }
</style>