<template>
    <section class="upper_header w-100">
        <div class="container">
            <div class="row row-cols-2">
                <div class="col">
                    <div class="day_wrapper text-start">
                        <h3 class="mb-0">{{dayTime.currentDay}}</h3>
                    </div>
                </div>
                <div class="col">
                    <div class="time_wrapper text-end">
                        <h3 class="mb-0">{{dayTime.currentTime}}</h3>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<script>


export default {
    name: 'UpperHeader',
    props: {
        dayTime: Object
    }
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';

    /* Caratteristiche upper header */
    .upper_header{
        height: calc(100% / 3);
        color: $primary_color;
    }
</style>