<template>
    <!-- Griglia di gioco contenente tutte le celle, ha il background corrispondente a quello del livello attuale
    La classe delle celle varia a seconda degli indici di celle come lava,electro,toxic etc. se gli indici della cella
    corrispondono con uno di quelle, si colora del relativo colore 
    (cyan per start, blue per end, rainbow per teleport, yellow per electric, red per lava, lime per toxic e bg_toxic nel caso l'utente si trovi sulla cella toxic) -->
    <div class="grid w-100 h_70 d-flex flex-wrap position-relative" :class="'bg_' + currentLevel.background">
        <Cell v-for="cell,index in cells.total" :key="index" :cellIndex="index" :userPosition="currentLevel.indexes.user" :alienPosition="currentLevel.indexes.alien.path[currentLevel.indexes.alien.current]" 
        :currentCharacterClass="'fas fa-' + currentCharacter.icon + ' text-' + currentCharacter.color" :gameStatus="statusCopy.game" :levelsStatus="statusCopy.levels"
        :class="{'bg_cyan': index === currentLevel.indexes.start,
                 'bg_blue': index === currentLevel.indexes.end,
                 'bg_rainbow': currentLevel.indexes.teleports.indexes.includes(index),
                 'bg_yellow': currentLevel.indexes.electric.indexes.includes(index),
                 'bg_red': currentLevel.indexes.lava.includes(index),
                 'bg_lime': currentLevel.indexes.toxic.indexes.includes(index),
                 'bg_toxic': currentLevel.indexes.toxic.indexes.includes(currentPosition) && currentPosition === index 
                }"      
        />
        <!-- Titolo da stampare solo nella sezione levels (nel caso di levels, currentLevel non è quello attuale ma quello selezionato) -->
        <LockedOverlay v-if="!currentLevel.unlocked" />
    </div>
</template>

<script>
import Cell from './Cell.vue';
import LockedOverlay from './LockedOverlay.vue';

export default {
    name: 'Grid',
    components: {
        Cell,
        LockedOverlay
    },
    props: {
        cells: Object,
        borders: Object,
        currentLevel: Object,
        currentCharacter: Object,
        currentDirection: String,
        currentPosition: Number,
        statusCopy: Object
    },
    methods: {
        // Funzione per definire i limiti della griglia, pusha negli array di Main
        // Non ha infatti numeri precisi ma lavora su variabili
        // Cicli: (asse x aumenta di 1, asse y aumenta di numero di celle per riga)
            // Nord: percorre gli indici da 0 al numero di celle per riga, così farà tutta la riga nord in alto
            // Est: percorre gli indici dal numero di celle per riga all'ultimo indice, così farà tutta la colonna est a destra
            // Sud: percorre gli indici dall'ultimo indice all'ultimo indice meno il numero di celle per riga, così farà tutta la riga sud in basso 
            // Ovest: percorre gli indici dall'ultimo meno il numero di celle per riga fino a 0, così farà tutta la colonna ovest a sinistra
        defineBorders: function(){
            for(let i = 0; i < this.cells.x; i++){
                this.borders.north.push(i);
            }
            for(let i = this.cells.x - 1; i <= this.cells.total - 1; i = i + this.cells.x){
                this.borders.east.push(i);
            }
            for(let i = this.cells.total - 1; i >= this.cells.total - this.cells.x; i--){
                this.borders.south.push(i);
            }
            for(let i = this.cells.total - this.cells.x; i >= 0; i = i - this.cells.x){
                this.borders.west.push(i);
            }
        }
    },
    created: function(){
        // Al created della griglia faccio partire la funzione per definire i limiti della griglia 
        this.defineBorders();
    }
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';

    .grid{
        border-radius: 5px;
        background-size: cover;
        background-position: center;
        background-repeat: no-repeat;

        /* Modifiche celle nella griglia */
        .cell{
            width: calc(100% / $cells_x);
            height: calc(100% / $cells_y);
        }   
    }
</style>