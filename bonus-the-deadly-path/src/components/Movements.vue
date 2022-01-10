<template>
    <section class="movements">
        <h2>movements</h2>
        <div class="movement_types w-100 d-flex">
            <div v-for="movement,index in movements" :key="index" @click="pickMovement(movement)" class="move_type" :class="{'selected': current.movement === movement}">
                <GamePad v-if="movement === 'gamepad'" />
                <h3 v-else class="fw-bold text-uppercase mb-0">{{movement}}</h3>
            </div>  
        </div>
        <Button @click.native="leaveSection" :msg="'choose'" /> 
    </section>
</template>

<script>
import GamePad from './GamePad.vue';
import Button from './Button.vue';

export default {
    name: 'Movements',
    components: {
        GamePad,
        Button
    },
    props: {
        status: Object,
        movements: Array,
        current: Object
    },
    methods: {
        // Funzione per passare alla sezione successiva
        leaveSection: function(){
            this.status.movements = false;
            this.status.characters = true;
        },
        // Funzione per cambiare il movimento selezionato e aggiornare il movimento attuale per l'utente
        pickMovement: function(clickedMovement){
            this.current.movement = clickedMovement;
        },
    }
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';

    .movement_types{
        gap: 1rem;

        /* Modifiche size per i box con i tipi di movimento */
        .move_type{
            width: calc(100% / 3);
            display: flex;
            justify-content: center;
            align-items: center;
            border: 5px solid $secondary_color;
            padding: 1rem;
            border-radius: 20px;
            cursor: pointer;
            position: relative;

            &.selected{
                animation: selected_item 1s linear infinite;
            }
            .gamepad{
                width: 100%;
                height: 150px;
                font-size: 40px;
            }
        }
    }
</style>