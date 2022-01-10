<template>
    <nav class="navbar w-100">
        <div class="container">
            <img class="navbar-brand w_200p" src="../assets/img/logo_2_small.png" alt="Logo del gioco">
            <img class="w_50p" src="../assets/img/logo_small_icon_only_inverted.png" alt="Logo del gioco">
        </div>
    </nav>
</template>

<script>

export default {
    name: 'NavBar'
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';

    /* Modifiche Navbar */
    .navbar{
        color: $primary_color;   
    }
    .navbar-brand{
        width: 100px;
    }

    @media screen and (min-width: 576px){
        .navbar-brand{
            width: 120px;
        }
    }
    @media screen and (min-width: 768px){
        .navbar-brand{
            width: 140px;
        }
    }
    @media screen and (min-width: 992px){
        .navbar-brand{
            width: 170px;
        }
    }
    @media screen and (min-width: 1200px){
        .navbar-brand{
            width: 200px;
        }
    }
    @media screen and (min-width: 1400px){
        .navbar-brand{
            width: 250px;
        }
    }
</style>