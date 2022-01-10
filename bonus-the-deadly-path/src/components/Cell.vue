<template>
    <!-- Singola cella di gioco, la uso nel tutorial e griglia di gioco
    Width e height variano a seconda di dove lo stampo -->
    <div class="cell">
        <!-- Per evitare bug nella sezione Tutorial dove mostro le celle, user e alieno sono stampati solo se la sezione Game è true 
        Non uso v-if v-else per stampare user e alieno, ma uso 2 v-if per farli sovrapporre nel caso la posizione dei 2 coincida 
        Viene stampato l'user nella cella se l'indice della cella corrisponde con quello attuale dell'user  -->
        <User v-if="cellIndex === userPosition && (gameStatus || levelsStatus)" class="fs_20" :class="currentCharacterClass" />
        <!-- Viene stampato l'alieno nella cella se l'indice della cella corrisponde con quello attuale dell'alieno -->
        <Alien v-if="cellIndex === alienPosition && (gameStatus || levelsStatus)" class="fs_20" />
        <!-- Questo span è di aiuto per vedere l'indice della cella, sarà da eliminare quando ho finito tutto il programma, 
        viene stampato solo se dentro non c'è l'user o l'alieno così da evitare bug grafici -->
        <!-- <span v-else-if="gameStatus" class="text-white fw-bold">{{cellIndex}}</span> -->
    </div>
</template>

<script>
import User from './User.vue';
import Alien from './Alien.vue';

export default {
    name: 'Cell',
    components: {
        User,
        Alien
    },
    props: {
        cellIndex: Number,
        userPosition: Number,
        alienPosition: Number,
        currentCharacterClass: String,
        gameStatus: Boolean,
        levelsStatus: Boolean
    }
}
</script>

<style lang="scss" scoped>
    /* Customizzazione celle: gli elementi al suo interno saranno centrati con transform translate
    Poiché quando user e alieno sono sulla stessa cella, voglio far vedere l'alieno sopra l'user
    con z-index più alto per dare l'idea visiva che l'alieno abbia "preso" l'user */
    .cell{
        position: relative;

        > *{
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);

            span{
                z-index: 5;
            }
            .user_icon{
                z-index: 10;
            }
            .alien{
                z-index: 15;
            }
        }
    }
</style>
