<template>
    <!-- Sezione game, ha un titolo corrispondente al nome del livello attuale, la griglia di gioco e il gamepad 
    se l'utente ha scelto gamepad, altrimenti un titolo che ricorda all'utente quali tasti usare per muoversi -->
    <section class="game">
        <h2>{{levels[current.level].name}}</h2>
        <Grid :cells="grid.cells" :borders="grid.borders" :currentLevel="levels[current.level]" :currentCharacter="current.character"  
        :currentDirection="nextDirection" :currentPosition="levels[current.level].indexes.user" :statusCopy="status" 
        />
        <!-- Il GamePad viene stampato solo se la scelta dell'utente è quella del gamepad -->
        <h3 v-if="current.movement === 'arrows'">Use the arrows to move</h3>
        <h3 v-else-if="current.movement === 'w a s d'">Use W,A,S,D to move</h3>
        <GamePad v-else :gameStatus="status.game" @newDirection="getDirection($event)" />
    </section>
</template>

<script>
import Grid from './Grid.vue';
import GamePad from './GamePad.vue';

export default {
    name: 'Game',
    components: {
        Grid,
        GamePad
    },
    props: {
        status: Object,
        utilities: Object,
        grid: Object,
        current: Object,
        levels: Array
    },
    data: function(){
        return {
            // Variabile che serve per inquadrare la nuova posizione scelta dall'utente
            // Varia a seconda della modalità di movimento scelta
            nextDirection: null
        };
    },
    methods: {
        // Funzione che cambia la posizione dell'utente in base alla direzione scelta
        // Ogni cambio direzione avviene se l'array contenente i bordi e corrispondente alla direzione scelta 
        // non include la posizione attuale dell'utente, così non può andare fuori la mappa
        // La const previousIndex serve per registrare la posizione attuale dell'utente prima che la posizione cambi,
        // così da registrare la posizione precedente dell'utente ed inviarla a checkToxic
        switchDirection: function(direction){
            // Registro la posizione attuale dell'utente in una const, subito dopo la posizione cambia, quindi registro effettivamente la precedente posizione
            const previousIndex = this.levels[this.current.level].indexes.user;
            // Il movimento viene reso possibile solo se il gioco non è finito e l'utente non si sta teletrasportando
            if(!this.utilities.gameIsOver && !this.utilities.teleporting){
                switch(direction){
                    // Se la direzione è Up, l'indice diminuisce del numero di celle per riga
                    case 'Up':
                        if(!this.grid.borders.north.includes(this.levels[this.current.level].indexes.user)){
                            this.levels[this.current.level].indexes.user -= this.grid.cells.x;
                        }
                        break;
                    // Se la direzione è Right, l'indice aumenta di 1
                    case 'Right':
                        if(!this.grid.borders.east.includes(this.levels[this.current.level].indexes.user)){
                            this.levels[this.current.level].indexes.user++;
                        }
                        break;
                    // Se la direzione è Down, l'indice aumenta del numero di celle per riga
                    case 'Down':
                        if(!this.grid.borders.south.includes(this.levels[this.current.level].indexes.user)){
                            this.levels[this.current.level].indexes.user += this.grid.cells.x;
                        }
                        break;
                    // Se la direzione è Left, l'indice diminuisce di 1
                    case 'Left':
                        if(!this.grid.borders.west.includes(this.levels[this.current.level].indexes.user)){
                            this.levels[this.current.level].indexes.user--;
                        }
                        break;
                }
                // Richiamo alla funzione per annullare il setTimeout del gameover e per ripristinare i 2 secondi di conteggio sulla toxic
                this.checkToxic(previousIndex);
                // Richiamo la funzione che controlla la cella sulla quale l'utente si trova dopo il cambio posizione
                this.checkCell();
            }
        },
        // Funzione legata all'emit di GamePad per ricevere la nuova direzione scelta dall'utente
        getDirection: function(newDirection){
            this.nextDirection = newDirection;
            // Richiamo la funzione per aggiornare Main sulla nuova posizione dell'utente
            this.switchDirection(this.nextDirection);
        },
        // Funzione legata all'emvento aggiunto a window per il press su wasd se la scelta è quella, altrimenti sulle frecce della tastiera
        // Il primo switch è nel caso la scelta sia wasd,  il secondo se è arrows cioè le frecce della tastiera
        getDirection2: function(keyEvent){
            // Switch Case sul tipo di scelta tra wasd e arrows
            switch(this.current.movement){
                case 'w a s d':
                    // Switch case sul tipo di tasto premuto nella scelta wasd
                    switch(keyEvent.key){
                    case 'w':
                        this.nextDirection = 'Up';
                        break;
                    case 'd':
                        this.nextDirection = 'Right';
                        break;
                    case 's':
                        this.nextDirection = 'Down';
                        break;
                    case 'a':
                        this.nextDirection = 'Left';
                        break;
                    }
                    break;
                case 'arrows':
                    // Switch case sul tipo di tasto premuto nella scelta arrows
                    switch(keyEvent.key){
                    case 'ArrowUp':
                        this.nextDirection = 'Up';
                        break;
                    case 'ArrowRight':
                        this.nextDirection = 'Right';
                        break;
                    case 'ArrowDown':
                        this.nextDirection = 'Down';
                        break;
                    case 'ArrowLeft':
                        this.nextDirection = 'Left';
                        break;
                    }
                    break;
            }  
            // Richiamo la funzione per aggiornare Main sulla nuova posizione dell'utente
            this.switchDirection(this.nextDirection);
            // Debug: dopo aver inviato la nuova posizione, la direction diventa null, così da darle poi un nuovo valore al press di uno degli 8 tasti movimento
            // Senza questa cosa, ad esempio la direzione resterebbe sempre "Up" ed anche al press di un altro pulsante qualsiasi si muoverebbe verso l'alto
            // Con questo reset quindi mi assicuro che il movimento avvenga solo premendo uno degli 8 tasti interessati ed altri tasti non abbiano influenza
            this.nextDirection = null;
        },
        // Funzione per gestire l'apparizione/sparizione delle celle electric ogni 2 secondi
        // L'array contenente gli indici delle celle electro verrà uguagliato alla copia al v-if del Game, quindi c'è già il reset
        // Uso una variabile per inquadrare l'azione corrente, cancellando o eliminando così da fare un effetto toggle
        // Non uso lo splice altrimenti andrebbe a modificarmi l'array, ma eguaglio l'array che si deve svuotare direttamente ad un array vuoto []
        // Questo accade solo se deleting è true, altrimenti eguaglio l'array alla sua copia, così da ripristinarne il contenuto
        // L'effetto ottenuto è che ogni 2 secondi la cella electro sparisce, poi dopo altri 2 secondi ricompare
        electricToggle: function(){
            // Booleana per inquadrare l'azione attuale
            let deleting = true;
            // Variabile tempo
            this.levels[this.current.level].indexes.electric.clock = setInterval(() => {
                // Azione dello svuotare l'array
                if(deleting){
                    // L'array diventa letteralmente un array vuoto
                    this.levels[this.current.level].indexes.electric.indexes = [];
                    // Deleting va false
                    deleting = false;
                } else{
                    // L'array diventa uguale alla sua copia
                    this.levels[this.current.level].indexes.electric.indexes = this.levels[this.current.level].indexes.electric.copy;
                    // Deleting va false
                    deleting = true;
                }
                // Condizione per il game over se la cella compare sotto all'utente mentre è fermo
                if(this.levels[this.current.level].indexes.electric.indexes.includes(this.levels[this.current.level].indexes.user)){
                this.gameOver('Game Over!', 'An Electric cell electrocuted you!');
                }
            }, 2000);
        },
        // Funzione che cambierà ogni secondo la posizione dell'alieno del livello corrente così da comunicarlo a Game->Grid->Cell
        // La posizione corrente (current) è già impostata col valore del primo numero dell'array path
        // Cambio posizione: dopo 1 secondo la posizione attuale sarà quella successiva dell'array path
        // Se arriva all'ultima, torna indietro percorrendolo al contrario per poi ricominciare a percorrerlo normalmente e così via
        // Inoltre se l'alieno cammina nella cella dove si trova l'utente, il gioco finisce
        alienWalks: function(){
            // Dichiaro questa variabile booleana fuori dalla funzione asincrona
            // quindi sarà true solo al richiamo della funzione, poi sarà cambiata ogni secondo
            let increasing = true;
            // La prima volta increasing è true, quindi legge la prima condizione
            this.levels[this.current.level].indexes.alien.clock = setInterval(() => {
                if(increasing){
                    // Aumento l'indice della posizione attuale dell'alieno in corrispondenza dell'array path di 1
                    this.levels[this.current.level].indexes.alien.current++;
                    // Se arrivo all'ultimo indice dell'array path, torno indietro fino al primo poiché increasing diventa false
                    if(this.levels[this.current.level].indexes.alien.current === this.levels[this.current.level].indexes.alien.path.length - 1){
                        increasing = false;
                    }
                } else{
                    // Diminuisco l'indice della posizione attuale dell'alieno in corrispondenza dell'array path di 1
                    this.levels[this.current.level].indexes.alien.current--;
                    // Se arrivo al primo indice dell'array path, ricomincio a salire poiché increasing diventa true
                    if(this.levels[this.current.level].indexes.alien.current === 0){
                        increasing = true;
                    }
                }
                // Condizione per il game over se l'alieno cammina sulla cella in cui si trova l'utente, quindi se le posizioni dei 2 coincidono
                if(this.levels[this.current.level].indexes.user === this.levels[this.current.level].indexes.alien.path[this.levels[this.current.level].indexes.alien.current]){
                    this.gameOver('Game Over!','You were caught by the Alien!');
                }
            }, 1000);
        },
        // Funzione per gestire il teletrasporto, dura 1 secondo e impedisce all'utente di muoversi durante quel secondo
        // Gestisce completamente una booleana e un'altra per metà, 
        //la prima per non far muovere l'utente durante il teletrasporto
        // la seconda per far capire al gioco che l'utente si è appena teletrasportato, quindi se va verso un muro e il teletrasporto è lungo i bordi della griglia,
        // non avverrà di nuovo il teletrasporto in quanto l'utente di fatto non si è mosso ma la posizione viene aggiornata con quella attuale, quindi senza questa variabile si teletrasporterebbe di nuovo
        teleportTime: function(){
            this.utilities.teleporting = true;
            this.levels[this.current.level].indexes.teleports.clock = setTimeout(() => {
                // Switch Case applicato sulla posizione attuale dell'utente
                switch(this.levels[this.current.level].indexes.user){
                    // Caso cella teletrasporto 1: se la posizione attuale dell'utente corrisponde con quella del primo teletrasporto, diventa quella del secondo
                    case this.levels[this.current.level].indexes.teleports.indexes[0]:
                        this.levels[this.current.level].indexes.user = this.levels[this.current.level].indexes.teleports.indexes[1];
                        break;
                    // Caso cella teletrasporto 2: se la posizione attuale dell'utente corrisponde con quella del secondo teletrasporto, diventa quella del primo
                    case this.levels[this.current.level].indexes.teleports.indexes[1]:
                        this.levels[this.current.level].indexes.user = this.levels[this.current.level].indexes.teleports.indexes[0];
                        break;
                }
                // La booleana per non far muovere l'utente durante il secondo del teletrasporto va false
                this.utilities.teleporting = false;
                // La booleana per capire se l'utente si è appena teletrasportato diventa true, verrà resa false con l'if iniziale della funzione checkCell
                this.utilities.justTeleported = true;
            }, 1000);
        },
        // Funzione che attiverà i 2 secondi per il gameover se l'utente si trova sulla cella toxic
        toxicTime: function(){
            this.levels[this.current.level].indexes.toxic.clock = setTimeout(() => {
                this.gameOver('Game Over!', 'You were poisoned by a Toxic cell!');
            }, 2000);
        },
        // Funzione per controllare la cella sulla quale si trova l'utente
        // La funzione sarà richiamata dopo il cambio posizione dell'utente, 
        // quindi prende in considerazione la posizione attuale dell'utente (invece che la prossima), poiché è già cambiata
        // Non uso switch case poiché in alcuni casi il controllo è l'includes, in altri è l'uguaglianza
        // Gestirà questi casi:
            // User sulla cella finale (game won)
            // User su una cella teleports (teletrasporto da un index all'altro)***
            // User sulla cella electric (game over)
            // User sulla cella lava (game over)
            // User sulla cella toxic (game over)
            // User sulla stessa cella dell'alieno (game over)
        // ***Debug da risolvere: se un teletrasporto è lungo i bordi della griglia e l'utente ci si è appena teletrasportato, 
        // se va verso il bordo non cambia la posizione(corretto) ma la nuova posizione è la stessa, quindi si teletrasporta di nuovo anche se in realtà non si muove
        // Per risolvere questo bug, userò la booleana justTeleported: nel primo if della funzione, specifico che deve diventare false quando l'utente cammina
        // In qualsiasi cella che non sia un teletrasporto. quando l'utente si è teletrasportato, diventa true
        // Il teletrasporto è possibile solo se justTeleported è false, quindi risolverà il bug del teletrasporto su una cella al bordo e movimento verso il bordo
        checkCell: function(){
            if(!this.levels[this.current.level].indexes.teleports.indexes.includes(this.levels[this.current.level].indexes.user)){
                // La variabile booleana per capire se l'utente si è appena teletrasportato diventa false
                this.utilities.justTeleported = false;
            }
            // Caso cella end: se la posizione attuale dell'utente è uguale a quella della cella finale, Game Won
            if(this.levels[this.current.level].indexes.user === this.levels[this.current.level].indexes.end){
                this.gameOver('You Won!', 'You made it to the end!');
              // Caso cella teletrasporto: Si divide in 2 casi, i teletrasporti sono sempre 2 quindi posso usare 0 e 1
              // Il caso è gestito se l'array contenente gli indici dei teletrasporti include la posizione attuale dell'utente e justTeleported è false 
              // Tutto ciò avviene nella funzione teleportTime
            } else if(this.levels[this.current.level].indexes.teleports.indexes.includes(this.levels[this.current.level].indexes.user) && !this.utilities.justTeleported){
                this.teleportTime();
              // Caso cella electric: se l'array contenente gli indici delle celle electric include la posizione attuale dell'utente, Game Over   
            } else if(this.levels[this.current.level].indexes.electric.indexes.includes(this.levels[this.current.level].indexes.user)){
                this.gameOver('Game Over!', 'An Electric cell electrocuted you!');
              // Caso cella lava: se l'array contenente gli indici delle celle lava include la posizione attuale dell'utente, Game Over  
            } else if (this.levels[this.current.level].indexes.lava.includes(this.levels[this.current.level].indexes.user)){
                this.gameOver('Game Over!', 'You walked on a Lava Cell!');
              // Caso cella toxic: se l'array contenente gli indici delle celle toxic include la posizione attuale dell'utente, Game Over (funzione che da il game over dopo 2 secondi sulla cella toxic) 
            } else if(this.levels[this.current.level].indexes.toxic.indexes.includes(this.levels[this.current.level].indexes.user)){
                this.toxicTime();
              // Caso alieno: se la posizione dell'utente corrisponde con quella dell'indice attuale dell'array path (alieno), Game Over  
            } else if(this.levels[this.current.level].indexes.user === this.levels[this.current.level].indexes.alien.path[this.levels[this.current.level].indexes.alien.current]){
                this.gameOver('Game Over!', 'You were caught by the Alien!');
            }
        },
        // Funzione richiamata ad ogni switch movement per fermare il setTimeout di toxicTime:
        // In questo modo, se l'utente esce dalla cella toxic, si annulla il gameover con il clearTimeout
        // Inoltre, se l'utente esce dalla cella, il clearTimeout fermerà anche i 2 secondi dopo i quali la toxic diventa deadly
        // Quindi se dopo 2 secondi l'utente torna sulla toxic, non si becca il gameover istantaneo, ma fa ripartire quei 2 secondi, appena esce li ferma e così via
        // Uso il parametro previous che è la posizione immediatamente precedente dell'utente, così posso paragonarlo all'attuale
        checkToxic: function(previous){
            if(this.levels[this.current.level].indexes.user !== previous){
                clearTimeout(this.levels[this.current.level].indexes.toxic.clock);
            }
        },
        // Funzione per passare alla sezione GameOver (nel caso di vittoria o sconfitta)
        // Primo parametro: vittoria/sconfitta
        // Secondo parametro: descrizione della sconfitta/vittoria
        gameOver: function(finalResult, finalDescription){
            // La booleana per impedire il movimento durante il secondo fino a end game diventa true
            this.utilities.gameIsOver = true;
            // I valori di gameResult diventano i valori ricevuti come parametro, sottoforma di stringa
            this.utilities.gameResult.result = finalResult;
            this.utilities.gameResult.description = finalDescription;
            // Fermo le funzioni tempo di electric cell e alieno, e toxic per evitare bug nel titolo finale
            clearInterval(this.levels[this.current.level].indexes.electric.clock);
            clearInterval(this.levels[this.current.level].indexes.alien.clock);
            clearTimeout(this.levels[this.current.level].indexes.toxic.clock);
            // Dopo 1s mostro la schermata endgame
            this.utilities.gameOverClock = setTimeout(() => {
                // Resetto la posizione di utente e alieno e l'array degli indici delle celle electro con il valore della copia
                this.levels[this.current.level].indexes.electric.indexes = this.levels[this.current.level].indexes.electric.copy;
                this.levels[this.current.level].indexes.user = this.levels[this.current.level].indexes.start;
                this.levels[this.current.level].indexes.alien.current = 0;
                // Richiamo la funzione che sblocca il livello successivo 
                // (se il risultato è You Won e se non mi trovo all'ultimo livello, altrimenti sbloccherei un livello che non esiste)
                if(finalResult === 'You Won!' && this.current.level < this.levels.length - 1){
                    this.unlockLevel();
                }
                this.status.game = false;
                this.status.endgame = true;
                // Rimuovo l'evento per usare le frecce quando Game non viene più stampato*
                if(this.current.movement !== 'gamepad'){
                    window.removeEventListener('keyup', this.getDirection2);
                }
            }, 1000);
        },
        // Funzione che gestisce lo sblocco del livello successivo (se si è completato l'attuale)
        unlockLevel: function(){
            this.levels[this.current.level + 1].unlocked = true;
        },  
        // Funzione per resettare i valori utilities quando game viene stampato di nuovo
        resetGame: function(){
            // La copia dell'array degli indici electric viene eguagliata a questo array prima che diventi un array vuoto, così da rendere possibile il toggle
            this.levels[this.current.level].indexes.electric.copy = this.levels[this.current.level].indexes.electric.indexes;
            // Setto false la variabile per debuggare il teletrasporto
            this.utilities.justTeleported = false;
            // Resetto la posizione di utente e alieno
            this.levels[this.current.level].indexes.user = this.levels[this.current.level].indexes.start;
            this.levels[this.current.level].indexes.alien.current = 0;
            // Parte la funzione toggle per le celle electric
            this.electricToggle();
            // Parte la funzione che sposta l'alieno
            this.alienWalks();
            // La booleana per impedire il movimento al game over diventa nuovamente false per reset
            this.utilities.gameIsOver = false;
            // Resetto i valori di game result
            this.utilities.gameResult.result = null;
            this.utilities.gameResult.description = null;
        }
    },
    created: function(){
        // Resetto il gioco
        this.resetGame();
        // Evento legato alla pagina per rendere possibile il movimento con le frecce o con wasd 
        // (l'evento viene legato alla pagina solo se mi trovo nella sezione Game* e l'utente non ha scelto gamepad)
        if(this.status.game && this.current.movement !== 'gamepad'){
            window.addEventListener('keyup', this.getDirection2);
        }
        
    }
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';

    
    .gamepad{
        width: 50%;
        height: 15%;
        font-size: 40px;
    }

    @media screen and (min-width: 576px){
        .gamepad{
            font-size: 50px;
        }
    }
    @media screen and (min-width: 768px){
        .gamepad{
            font-size: 50px;
        }
    }
    @media screen and (min-width: 992px){
        .gamepad{
            width: 35%;
            height: 20%;
            font-size: 60px;
        }
    }
    @media screen and (min-width: 1200px){
        .gamepad{
            font-size: 70px;
        }
    }

</style>