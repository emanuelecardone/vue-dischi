<template>
<!-- La width va data come classe al richiamo -->
    <div class="decoration d-flex justify-content-center align-items-center">
        <div class="left_decoration line"></div>
        <i class="star_decoration fas fa-star"></i>
        <div class="right_decoration line"></div>
    </div>
</template>

<script>
export default {
    name: 'Decoration'
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';

    /* Customizzazione della decorazione */
    .decoration{
        color: $primary_color;
        gap: 1rem;

        .line{
            border: 2px solid $primary_color;
            background-color: $primary_color;
            border-radius: 20px;
            width: 50px;
        }
        .fa-star{
            font-size: 27.5px;
        }
    }

    /* Media per la decorazione (devo usare la stessa struttura innestata altrimenti non sovrascrive perché meno specifica di prima) */
    @media screen and (min-width: 576px){
        .decoration{
            .line{
                width: 70px;
            }
        }
    }
    @media screen and (min-width: 768px){
        .decoration{
            .line{
                width: 90px;
            }
        }
    }
    @media screen and (min-width: 992px){
        .decoration{
            .line{
                width: 110px;
            }
        }
    }
    @media screen and (min-width: 1200px){
        .decoration{
            .line{
                width: 130px;
            }
        }
    }
    @media screen and (min-width: 1400px){
        .decoration{
            .line{
                width: 150px;
            }
        }
    }
</style>