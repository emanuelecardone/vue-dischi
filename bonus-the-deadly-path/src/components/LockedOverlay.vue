<template>
    <div class="locked_overlay w-100 h-100 position-absolute">
        <h3 class="locked_text mb-0 position-absolute">Locked</h3>
    </div>
</template>

<script>
export default {
    name: 'LockedOverlay'
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';

    .locked_overlay{
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        background-color: rgba($color: black, $alpha: .8);

        /* Modifiche testo locked per la sezione Levels */
        .locked_text{
            color: $tertiary_color;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
            z-index: 2000;
            text-transform: uppercase;
            font-size: 40px;
            font-weight: bold;
            letter-spacing: 1rem;
            text-indent: 1rem;
        }
    }
    
</style>