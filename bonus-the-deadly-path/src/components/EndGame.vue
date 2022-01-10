<template>
    <!-- Sezione di fine gioco, compare nel caso di sconfitta o vittoria e il titolo che stampa corrisponde 
    a vittoria o sconfitta e, nel caso di sconfitta, comunica quale cella ha fatto perdere l'utente, o se è stato l'alieno -->
    <section class="endgame">
        <h2>{{finalMsg.result}}</h2>
        <h3 class="fw-bold">{{finalMsg.description}}</h3>
        <div class="buttons_container">
            <Button v-if="finalMsg.result === 'You Won!' && current.level < levels.length - 1" @click.native="leaveSection('next')" :msg="'Next Level'" />
            <Button @click.native="leaveSection('rechoose')" :msg="'Choose Level'" />
            <Button @click.native="leaveSection('restart')" :msg="'Restart'" />
            <Button @click.native="leaveSection('menu')" :msg="'Main Menù'" />
        </div>
    </section>
</template>

<script>
import Button from './Button.vue';

export default {
    name: 'EndGame',
    components: {
        Button
    },
    props: {
        status: Object,
        finalMsg: Object,
        current: Object,
        levels: Array
    },
    methods: {
        // Funzione per gestire i pulsanti di endgame (possibilità di riavviare il livello, sceglierne uno diverso o andare al menù principale)
        leaveSection: function(choice){
            // Lo status di endgame va false a prescindere
            this.status.endgame = false;
            // Switch Case sulla scelta dell'utente (parametro preso come stringa)
            switch(choice){
                case 'next':
                    if(this.levels[this.current.level + 1].unlocked){
                        // Il livello attuale diventa quello selezionato 
                        this.current.level++;
                        this.status.game = true;  
                    }
                    break;
                case 'rechoose':
                    this.status.levels = true;
                    break;
                case 'restart':
                    this.status.game = true;
                    break;
                case 'menu':
                    this.status.welcome = true;
                    break;
            }
        }
    }
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';


</style>