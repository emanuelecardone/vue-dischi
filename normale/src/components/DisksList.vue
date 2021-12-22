<template>
    <section class="diskslist_wrapper">
        <div class="container">
            <div class="row row-cols-5">
                <div class="col">
                    <Disk />
                </div>
            </div>
        </div>
    </section>
</template>

<script>
import Disk from './Disk.vue';

export default {
    name: 'DisksList',
    components: {
        Disk
    }
}
</script>

<style lang="scss" scoped>

</style>