<template>
    <!-- Sezione tutorial, mostra tutti gli elementi del gioco (la classe dell'user del tutorial sarà fas fa-sun text-primary di default, poi verrà cambiato dopo dall'utente) -->
    <section class="tutorial">
        <h2>tutorial</h2>
        <div class="tutorial_elements">
            <ul class="game p-0">
                <li><Cell class="bg_cyan" />Start cell</li>
                <li><Cell class="bg_blue" />End cell</li>
                <li><Cell class="bg_rainbow" />Teleport cell, it teleports you</li>
                <li><Cell class="bg_yellow" />Electric cell, it appears and disappears</li>
                <li><Cell class="bg_red" />Lava cell, it's static but deadly</li>
                <li><Cell class="bg_lime" />Toxic cell, it's deadly after standing 2 seconds on it</li>
                <li><Alien /> Alien, it moves around the map, don't let it catch you</li>
            </ul>
            <ul class="user p-0">
                <li>Your character. You can change it later <User class="fas fa-sun text-primary" /></li>
                <li>The gamepad, you can also play with W,A,S,D or arrows <GamePad /></li>
            </ul>
        </div>
        <Button @click.native="leaveSection" :msg="'Okay'" />
    </section>
</template>

<script>
import Cell from './Cell.vue';
import Alien from './Alien.vue';
import User from './User.vue';
import GamePad from './GamePad.vue';
import Button from './Button.vue';

export default {
    name: 'Tutorial',
    components: {
        Cell,
        Alien,
        User,
        GamePad,
        Button
    },
    props: {
        status: Object
    },
    methods: {
        // Funzione per passare alla sezione successiva
        leaveSection: function(){
            this.status.tutorial = false;
            this.status.movements = true;
        }
    }
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';
    
    .tutorial{
        
        /* Modifiche wrapper degli elementi */
        &_elements{
            width: 100%;
            display: flex;
            justify-content: space-between;
            align-items: center;

            /* Modifiche ul generiche*/
            ul{ 
                display: flex;
                flex-direction: column;
                align-items: flex-start;
                justify-content: center;
                gap: 1rem;

                /* Modifiche li generiche */
                li{
                    font-size: 17.5px;
                    font-weight: bold;
                    display: flex;
                    justify-content: center;
                    align-items: center;
                    gap: .5rem;
                }
                /* Modifiche li a sinistra */
                &.game{

                    li{
                        /* Width e height per ogni primo elemento delle li a sinsitra, celle e alieno */
                        > *:first-child{
                            width: 40px;
                            
                            &.cell{
                                height: 40px;
                                flex-shrink: 0;
                            }
                            &.alien{
                                font-size: 32.5px;
                            }
                        }
                    }
                }
                /* Modifiche li a destra */
                &.user{

                    li{
                        align-self: flex-end;
                        /* Width e height per ogni primo elemento delle li a sinsitra, celle e alieno */
                        > *:last-child{
                            
                            &.user_icon{
                                font-size: 40px;
                                margin-left: 30px;
                                margin-right: 30px;
                            }
                            &.gamepad{
                                width: 100px;
                                height: 60px;
                                font-size: 20px;
                            }
                        }
                    }
                }
            }
        }
    }
</style>