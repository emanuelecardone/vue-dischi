<template>
    <!-- Non divido il main in sotto-componenti in quanto non so ancora come gestire il valore delle variabili booleane come gameStarted in Main dai sotto-componenti -->
    <main class="d-flex justify-content-center align-items-center">

        <!-- Start Game -->
        <div v-if="!gameStarted" class="startgame_wrapper d-flex flex-column justify-content-center align-items-center text-uppercase text-white">
            <div class="startgame_text d-flex flex-column align-items-center">
                <h1 class="mb_20">Benvenuto/a</h1>
                <!-- Il valore del change della select andrà ad aggiornare l'indice della difficoltà tramite il v-model -->
                <select v-model="difficultyIndex" class="form-select form-select-md text_emerald fw-bold">
                    <option value="0">Easy</option>
                    <option value="1">Medium</option>
                    <option value="2">Hard</option>
                </select>
            </div>
            <button @click="generateGame" class="btn text-center text_emerald fw-bold text-uppercase bg-white py-3 px-4">start</button>
            <div class="records_wrapper">
                <h3 class="mt_40 mb_20 text-center">I tuoi record attuali<br>(Saranno aggiornati ad ogni fine partita)</h3>
                <!-- La lista aggiornerà sempre i 3 record ad ogni fine partita, e verranno visualizzati tutti al click di "Ricomincia" -->
                <ul class="d-flex flex-column justify-content-center align-items-center p-0">
                    <li v-for="(value, key, index) in userRecord" :key="index">{{key}}: {{value}}</li>
                </ul>
            </div>
        </div>

        <!-- Game Wrapper -->
        <div v-else-if="gameStarted && !showEndGame" class="game_wrapper d-flex flex-wrap">
            <!-- Al click, se clicko un Red si colorano di rosso tutti i red, se clicko un Green allora solo quel green si colora di verde,
             inoltre decido width e height della col in base alla difficoltà scelta tramite la props di GameCol -->
            <GameCol v-for="(number, index) in colsAmount" :key="index" :colNumber="number.toString()" :colSize="colSizeClass" @click.native="revealCol(number)" :class="{'bg_red': redIsClicked && redsArray.includes(number), 'bg_lime': greenIsClicked && clickedGreens.includes(number)}" />
        </div>

        <!-- Game Over -->
        <div v-else class="gameover_wrapper d-flex flex-column justify-content-center align-items-center text-center text-white">
            <h2 class="text-uppercase mb-0">{{gameOverMessage}}</h2>
            <h3 class="mb-0">Hai totalizzato: {{userScore}} punti</h3>
            <!-- Verrà mostrato il record (specificando di quale difficoltà) corrispondente alla difficoltà scelta -->
            <h3 class="mb-0">Il tuo record ({{colSizeClass}}) è {{recordToShow}}</h3>
            <button @click="backToMenu" class="btn text-center text_emerald fw-bold text-uppercase bg-white py-3 px-4">Ricomincia</button>
        </div>

    </main>
</template>

<script>
import GameCol from './GameCol.vue';

export default {
    name: 'Main',
    components: {
        GameCol
    },
    data: function(){
        return {
            gameStarted: false,
            difficultyIndex : '0',
            colSizeClass: null,
            colsAmount: null,
            redsAmount: 16,
            greensAmount: null,
            redsArray: [],
            greensArray: [],
            clickedGreens: [],
            redIsClicked: false,
            greenIsClicked: false,
            gameOver: false,
            gameOverMessage: null,
            gameOverClock: null,
            userScore: 0,
            // userScores è una matrice con 3 array, il primo contiene i record Easy, il secondo quelli Medium, il terzo quelli Hard
            // I vari record saranno pushati ad ogni fine partita nel sottoarray [0],[1] o [2] in base alla difficoltà attuale
            userScores: [
                [], [], []
            ],
            // userRecord è un oggetto che avrà come valore il valore più alto per ogni difficoltà di userScores (applicando Math.max.apply)
            userRecord: {
                easy: 0,
                medium: 0,
                hard: 0
            },
            // Record to show sarà il valore da mostrare a fine partita, in base alla difficoltà attuale
            recordToShow: 0,
            showEndGame: false
        };
    },
    methods: {
        // Funzione che richiama il restart e la generazione di Reds e Greens
        generateGame: function(){
            this.restartGame();
            this.difficultySettings();
            this.generateReds();
            this.generateGreens();
        },
        // Funzione che decide il numero di col e Reds in base alla difficoltà scelta
        difficultySettings: function(){
            switch(this.difficultyIndex){
                case '0':
                    this.colsAmount = 100;
                    this.colSizeClass = 'easy';
                    break;
                case '1':
                    this.colsAmount = 81;
                    this.colSizeClass = 'medium';
                    break;
                case '2':
                    this.colsAmount = 49;
                    this.colSizeClass = 'hard';
                    break; 
            }
            this.greensAmount = this.colsAmount - this.redsAmount;
        },
        // Funzione che genera i Reds
        generateReds: function(){
            while(this.redsArray.length < this.redsAmount){
                const randomNumber = Math.floor(Math.random() * this.colsAmount) + 1;
                if(!this.redsArray.includes(randomNumber)){
                    this.redsArray.push(randomNumber);
                }
            }
        },
        // Funzione che genera i Greens
        generateGreens: function(){
            for(let i = 1; i <= this.colsAmount; i++){
                if(!this.redsArray.includes(i)){
                    this.greensArray.push(i);
                }
            }
        },
        // Funzione per decidere se la col clickata è safe o meno
        revealCol: function(thisNumber){
            // Il click scatena l'evento solo se il gioco non è finito (non riuscendo a dare lo style event ignore all'elemento html)
            if(!this.gameOver){
                // Per decidere se ho perso
                if(this.redsArray.includes(thisNumber)){
                    // Variabile per colorare di rosso tutti i Red
                    this.redIsClicked = true;
                    // Il gioco finisce (serve ad annullare il click su tutte le col in quel secondo prima della schermata game over)
                    this.gameOver = true;
                    // Modifico il messaggio finale
                    this.gameOverMessage = 'Hai perso!';
                    // Dopo 1 secondo appare il wrapper con la schermata finale game over
                    this.gameOverClock = setTimeout(() => {
                        this.showEndGame = true;
                    }, 1000);
                } else{
                    // Variabile per colorare di verde il Green clickato
                    this.greenIsClicked = true;
                    // Il green viene contato solo se non è già presente nella lista dei clickati (altra sostituzione allo style event ignore)
                    if(!this.clickedGreens.includes(thisNumber)){
                        // Aggiungo all'array dei Green clickati
                        this.clickedGreens.push(thisNumber);
                        // Aumento lo score di 1
                        this.userScore++;
                        // Per decidere se ho vinto
                        if(this.clickedGreens.length === this.greensAmount){
                            // Il gioco finisce (serve ad annullare il click su tutte le col in quel secondo prima della schermata game over)
                            this.gameOver = true;
                            // Modifico il messaggio finale
                            this.gameOverMessage = 'Hai vinto!';
                            // Dopo 1 secondo appare il wrapper con la schermata finale game over
                            this.gameOverClock = setTimeout(() => {
                                this.showEndGame = true;
                            }, 1000);
                        }
                    }
                }

                // Aggiorno i record se è game over
                if(this.gameOver){
                    this.defineRecords();
                }
            } 
        },
        // Funzione per aggiornare i record in base alla difficoltà scelta aggiorno l'array userScores con indice difficultyIndex: 0(easy) / 1(medium) / 2(hard)
        // Ad ogni fine partita verrà aggiornata la lista dei record corrispondente alla difficoltà scelta, verrà mostrato il record corrispondente a quello della difficoltà scelta
        defineRecords: function(){
            this.userScores[this.difficultyIndex].push(this.userScore);
            for(let key in this.userRecord){
                if(key === this.colSizeClass){
                    this.userRecord[key] = Math.max.apply(null, this.userScores[this.difficultyIndex]);
                    this.recordToShow = this.userRecord[key];
                }
            }
        },
        // Funzione per tornare al menù iniziale cliccando su ricomincia
        backToMenu: function(){
            this.gameStarted = false;
        },
        // Funzione che svuota gli array e prepara tutto per il restart
        // Resetto: Reds, Greens, Greens clickati, variabili game started, game over e schermata finale, messaggio finale, 
        //          variabili per togliere il colore alle col clickate, score dell'utente
        restartGame: function(){
            this.redsArray.splice(0);
            this.greensArray.splice(0);
            this.clickedGreens.splice(0);
            this.gameStarted = true;
            this.gameOver = false;
            this.showEndGame = false;
            this.gameOverMessage = null;
            this.redIsClicked = false;
            this.greenIsClicked = false;
            this.userScore = 0;
        }
    }
    
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';

    main{
        height: calc(100vh - 200px);

        > div{
            width: $game_width_height;
            height: $game_width_height;
            
            &:not(.game_wrapper){
                gap: 30px;
            }
        }
        .game_wrapper{
            .game_col{
                border: 2px solid white;
                cursor: pointer;

                &.easy{
                    width: 10%;
                    height: 10%;
                }
                &.medium{
                    width: calc(100% / 9);
                    height: calc(100% / 9);
                }
                &.hard{
                    width: calc(100% / 7);
                    height: calc(100% / 7);
                }
            }
        }
    }
    @media screen and (max-height: 700px){
        main{
            height: 100vh;
        }
    }
    @media screen and (min-width: 992px) and (min-height: 992px){
        main{
            > div{
                width: calc($game_width_height + 300px);
                height: calc($game_width_height + 300px);
            }
        }
    }

</style>