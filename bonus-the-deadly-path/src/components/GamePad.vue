<template>
    <!-- Gamepad di gioco contenente le 4 frecce che "muovono il personaggio", in realtà cambiano l'indice della cella che deve stampare il personaggio
    Le dimensioni non verranno date qui visto che devono variare da Tutorial a Game, devo darle dopo -->
    <div class="gamepad position-relative">
        <i class="fas fa-arrow-circle-up y up" @click="nextDirection('Up')"></i>
        <i class="fas fa-arrow-circle-right x right" @click="nextDirection('Right')"></i>
        <i class="fas fa-arrow-circle-down y down" @click="nextDirection('Down')"></i>
        <i class="fas fa-arrow-circle-left x left" @click="nextDirection('Left')"></i>
    </div>
</template>

<script>
export default {
    name: 'GamePad',
    props: {
        gameStatus: Boolean
    },
    data: function(){
        return {
            
        };
    },
    methods: {
        // Funzione per definire la nuova direzione scelta dall'utente in base alla arrow clickata
        // Per evitare bug, il codice della funzione avviene solo se mi trovo nella sezione game
        // Altrimenti il cambio posizione avverrebbe anche nella sezione tutorial nel caso l'utente clickasse
        nextDirection: function(nextDirection){
            if(this.gameStatus){
                this.$emit('newDirection', nextDirection);
            }
        }
    }
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';

    /* Modifiche per posizionare le frecce */
    .gamepad{
        flex-shrink: 0;

        .fas{
            position: absolute;
            cursor: pointer;
            color: $secondary_color;

            &.y{
                left: 50%;
                transform: translateX(-50%);

                &.up{
                    top: 0;
                }
                &.down{
                    bottom: 0;
                }
            }
            &.x{
                top: 50%;
                transform: translateY(-50%);

                &.left{
                    left: 0;
                }
                &.right{
                    right: 0;
                }
            }
        }
    }
</style>