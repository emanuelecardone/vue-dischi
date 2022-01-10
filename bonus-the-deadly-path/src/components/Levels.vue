<template>
    <!-- Sezione Levels, permette all'utente di selezionare il livello (se sbloccato), funziona come una specie di slider,
    se si clicca una freccia cambia la preview, infatti la griglia che è importata come componente mostrerà la griglia del livello selezionato -->
    <section class="levels position-relative">
        <h2>levels</h2>
        <i class="fas fa-long-arrow-alt-left left position-absolute" @click="switchSelected('left')"></i>
        <div class="level w-75 h_70 position-relative">
            <h3 class="fw-bold">{{levels[current.selected].name}}</h3>
            <h4 class="fw-bold text-capitalize">{{levels[current.selected].difficulty}}</h4>
            <Grid :cells="grid.cells" :borders="grid.borders" :currentLevel="levels[current.selected]" :currentCharacter="current.character" :currentPosition="levels[current.selected].indexes.user" :statusCopy="status" />
        </div>
        <i class="fas fa-long-arrow-alt-right right position-absolute" @click="switchSelected('right')"></i>
        <Button @click.native="leaveSection" :msg="'Play'" />
    </section>
</template>

<script>
import Grid from './Grid.vue';
import Button from './Button.vue';

export default {
    name: 'Levels',
    components: {
        Grid,
        Button
    },
    props: {
        status: Object,
        levels: Array,
        current: Object,
        grid: Object 
    },
    methods: {
        // Funzione per passare alla sezione game e giocare il livello selezionato (se è sbloccato)
        leaveSection: function(){
            if(this.levels[this.current.selected].unlocked){
                this.status.levels = false;
                this.status.game = true;
                // Il livello attuale diventa quello selezionato 
                this.current.level = this.current.selected;  
            }
        },
        // Funzione per cambiare il livello selezionato da mostrare in preview per l'utente
        switchSelected: function(choice){
            switch(choice){
                case 'left':
                    this.current.selected = (this.current.selected > 0) ? this.current.selected - 1 : this.levels.length - 1;
                    break;
                case 'right':
                    this.current.selected = (this.current.selected < this.levels.length - 1) ? this.current.selected + 1 : 0;
            }
        }
    },
    created: function(){
        // current.selected diventa il livello attuale ogni volta che stampo Levels, così da riprendere la preview dal livello attuale
        this.current.selected = this.current.level;
        // Resetto l'array degli indici delle celle electro con il valore della copia nella sezione Levels per mostrare gli electro nella preview
        this.levels[this.current.level].indexes.electric.indexes = this.levels[this.current.level].indexes.electric.copy;
    }
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';

    /* Modifiche custom per il wrapper dei livelli */
    .level{
    border-radius: 20px;
    background-color: $secondary_color;
    color: $tertiary_color;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
    padding: 1rem;
            
        /* Griglia */
        .grid{
            overflow: hidden;
            border: 3px solid $tertiary_color;
        }
    }
    /* Locked text */
    .locked_text{
        color: $tertiary_color;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        z-index: 20;
    }
    /* Arrows */
    [class*="fa-long-arrow"]{
        top: 50%;
        transform: translateY(-50%);
        font-size: 40px;
        cursor: pointer;

        &.left{
            left: 1.5rem;
        }
        &.right{
            right: 1.5rem;
        }
    }

    /* Media arrows */
    @media screen and (min-width: 768px){
        [class*="fa-long-arrow"]{
            font-size: 50px;
            &.left{
                left: 2rem;
            }
            &.right{
                right: 2rem;
            }
        }
    }
    @media screen and (min-width: 992px){
        [class*="fa-long-arrow"]{
            font-size: 70px;
            &.left{
                left: 2.5rem;
            }
            &.right{
                right: 2.5rem;
            }
        }
    }
    @media screen and (min-width: 1200px){
        [class*="fa-long-arrow"]{
            font-size: 90px;
            &.left{
                left: 2.75rem;
            }
            &.right{
                right: 2.75rem;
            }
        }
    }
    @media screen and (min-width: 1400px){
        [class*="fa-long-arrow"]{
            font-size: 110px;
            &.left{
                left: 3rem;
            }
            &.right{
                right: 3rem;
            }
        }
    }

</style>