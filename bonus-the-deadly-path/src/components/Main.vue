<template>
    <main class="w-100 h-100">
        <div class="container h-100">
            <div class="row row-cols-1 h-100">
                <div class="col">
                    <Welcome v-if="datas.status.welcome" :status="datas.status" />
                    <Tutorial v-else-if="datas.status.tutorial" :status="datas.status" />
                    <Movements v-else-if="datas.status.movements" :status="datas.status" :movements="datas.user.movements" :current="datas.current" />
                    <Characters v-else-if="datas.status.characters" :status="datas.status" :user="datas.user" :current="datas.current" />
                    <Levels v-else-if="datas.status.levels" :status="datas.status" :levels="datas.levels" :current="datas.current" :grid="datas.grid" />
                    <Game v-else-if="datas.status.game" :status="datas.status" :utilities="datas.utilities" :grid="datas.grid" :current="datas.current" :levels="datas.levels" />
                    <EndGame v-else-if="datas.status.endgame" :status="datas.status" :finalMsg="datas.utilities.gameResult" :current="datas.current" :levels="datas.levels" />
                </div>
            </div>
        </div>
    </main>
</template>

<script>
import Welcome from './Welcome.vue';
import Tutorial from './Tutorial.vue';
import Movements from './Movements.vue';
import Characters from './Characters.vue';
import Levels from './Levels.vue';
import Game from './Game.vue';
import EndGame from './EndGame.vue';

export default {
    name: 'Main',
    components: {
        Welcome,
        Tutorial,
        Movements,
        Characters,
        Levels,
        Game,
        EndGame
    },
    props: {
        
    },
    data: function(){
        return {
            // Oggetto contenente tutte le info sul gioco
            datas:{
                // Status delle sezioni (per stabilire il v-if)
                // Da cambiare lo status se si vuole testare così stampa direttamente quella con true
                status: {
                    welcome: true,
                    tutorial: false,
                    movements: false,
                    characters: false,
                    levels: false,
                    game: false,
                    endgame: false
                },
                // Oggetto contenente variabili funzionali al gioco, come il clock del gameover e il justTeleported
                utilities: {
                    // Booleana per far capire a Main che il personaggio non può più spostarsi (mentre si sta teletrasportando)
                    teleporting: null,
                    // Booleana per debuggare il teletrasporto (verrà resa false quando viene stampato game così da resettarla ad ogni inizio livello)
                    justTeleported: null,
                    // Booleana per far capire a Main che il personaggio non può più spostarsi (quando il gioco finisce e passa 1 secondo per la schermata game over)
                    // (verrà resa false quando viene stampato game così da resettarla ad ogni inizio livello)
                    gameIsOver: null,
                    // Variabile tempo per gestire il game over che avviene dopo 1 secondo
                    gameOverClock: null,
                    // Oggetto che verrà riempito col risultato finale dell'utente in base a win/loss e in base a cosa ha fatto perdere l'utente se ha perso
                    gameResult: {
                        result: null,
                        description: null
                    }
                },
                // Oggetto con i possibili personaggi, colori e tipi di movimento
                user: {
                    icons: ['sun', 'anchor', 'biohazard', 'bug', 'chess-rook'],
                    colors: ['primary', 'secondary', 'warning', 'light', 'success'],
                    movements: ['arrows', 'w a s d', 'gamepad']
                },
                // Oggetto contenente i valori attuali per l'utente (come personaggio, livello etc), vanno 0 di default
                current: {
                    character: {
                        icon: 'sun',
                        color: 'primary'
                    },
                    movement: 'arrows',
                    level: 0,
                    selected: 0
                },
                // Oggetto contenente informazioni sulla griglia di gioco
                grid: {
                    cells: {
                        total: 500,
                        x: 25,
                        y: 20
                    },
                    // borders verrà riempito da Grid->Game al created
                    borders:{
                        north: [],
                        east: [],
                        south: [],
                        west: []
                    }
                },
                // Array contenente le informazioni su ciascun livello
                // Il primo livello è sbloccato di default
                levels: [
                    {
                        id: 1,
                        name: 'The Obsidian Room',
                        background: 'obsidian',
                        indexes: {
                            user: null,
                            start: 12,
                            end: 48,
                            teleports: {
                                indexes: [79,43],
                                clock: null
                            },
                            electric: {
                                indexes: [94,119,127,131,128,130,144,146,152,156,161,163,171,177,181,178,180,196,202,206,227,231,228,230,293,318,343,403,405],
                                copy: null,
                                clock: null
                            },
                            lava: [10,11,13,14,17,18,19,22,23,24,26,27,28,29,30,31,32,35,36,38,39,42,44,47,49,51,57,60,64,67,69,72,74,76,82,85,86,88,89,92,95,97,99,101,107,109,110,114,115,117,121,122,124,149,126,132,134,140,143,147,151,157,159,165,169,174,176,182,184,190,195,197,199,201,207,209,215,221,222,223,224,226,232,235,239,251,257,261,263,267,268,269,270,271,272,273,277,281,286,288,289,290,291,292,298,303,305,311,323,328,330,336,337,338,339,340,341,342,348,352,356,367,368,369,370,371,373,376,382,396,398,401,407,408,409,410,411,412,413,414,415,416,417,418,419,420,421,423,426,448,451,452,453,454,455,456,457,458,459,460,461,462,463,464,465,466,467,468,469,470,471,472,473],
                            toxic: {
                                indexes: [52,53,54,55,56,61,63,73,77,78,80,81,98,102,103,104,105,106,111,113,123,148,160,162,164,173,186,188,198,237,252,253,255,256,262,278,279,280,287,294,295,296,304,329,344,345,346,354,433,434,435,436,437,438,439,440,441,442,443,444,445],
                                clock: null
                            },
                            alien: {
                                current: 0,
                                path: [129,154,153,154,155,154,179,204,203,204,205,204,229],
                                clock: null
                            }
                        },
                        difficulty: 'easy',
                        key: null,
                        unlocked: true
                    },
                    {
                        id: 2,
                        name: 'The Magma Room',
                        background: 'lava',
                        indexes: {
                            user: null,
                            start: 386,
                            end: 351,
                            teleports: {
                                indexes: [353,473],
                                clock: null
                            },
                            electric: {
                                indexes: [56,59,62,65,68,71,186,193,329,363,364,379,388,389,419,421,422,436,444,447,446,469,471,472,494,496,497],
                                copy: null,
                                clock: null
                            },
                            lava: [1,2,3,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,51,74,76,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,99,101,122,124,126,127,130,131,132,134,135,136,137,138,141,142,143,144,146,147,149,151,152,153,154,155,157,158,159,163,164,165,166,171,172,174,176,196,199,201,203,204,205,207,208,209,213,214,215,216,220,221,222,224,226,228,229,230,231,232,233,234,235,236,237,238,239,240,241,242,243,244,245,246,247,249,251,253,272,274,276,279,281,282,283,284,285,286,287,288,289,290,291,292,293,294,295,296,297,299,302,305,306,324,325,326,327,328,331,333,334,335,336,337,338,340,342,344,345,346,347,348,349,350,354,356,360,361,362,365,367,375,376,377,378,381,385,387,390,391,392,393,394,395,396,397,398,402,405,406,409,410,412,413,423,424,426,430,431,432,433,434,438,439,440,441,442,448,449,451,467,474,476,477,478,479,480,481,482,483,484,485,486,487,488,489,490,491,492,498,499],
                            toxic: {
                                indexes: [54,55,57,58,60,61,63,64,66,67,69,70,72,73,98,121,123,148,160,161,162,167,168,169,173,197,198,210,211,212,217,218,219,223,248,273,298,303,304,320,321,322,323,357,358,359,382,383,384,399,403,404,407,408,416,417,435,437,460,461,462,463,464,465,466],
                                clock: null
                            },
                            alien: {
                                current: 0,
                                path: [178,179,180,181,182,183,184,185,186,187,188,189,190,191,192,193,194,195,170,145,120,119,118,117,116,115,114,113,112,111,110,109,108,107,106,105,104,103],
                                clock: null
                            }
                        },
                        difficulty: 'easy',
                        key: '12345',
                        unlocked: false
                    },
                    {
                        id: 3,
                        name: 'The Toxic Room',
                        background: 'toxic_level',
                        indexes: {
                            user: null,
                            start: 447,
                            end: 372,
                            teleports: {
                                indexes: [108,180],
                                clock: null
                            },
                            electric: {
                                indexes: [79,106,114,131,139,156,182,184,247,255,272,280,297,305,322,347,330,433,435,436,438,439,441,442,444,445],
                                copy: null,
                                clock: null
                            },
                            lava: [28,29,30,31,32,33,34,35,36,37,38,39,40,53,65,70,76,77,78,80,81,82,83,84,85,86,87,88,90,92,93,94,96,97,98,101,103,105,109,111,113,115,117,123,126,130,132,133,134,136,138,140,141,144,146,148,151,152,155,161,162,165,167,169,170,173,176,179,181,183,185,188,190,192,194,196,198,201,203,204,206,208,210,211,212,213,215,217,219,221,223,226,229,231,233,235,236,241,244,246,248,251,252,254,256,258,260,263,264,265,267,269,271,273,276,279,281,283,285,286,291,292,294,296,298,301,303,304,306,310,311,312,313,315,316,317,319,321,323,326,329,331,335,338,345,346,348,351,352,354,356,357,359,363,365,366,367,368,369,371,373,376,379,381,382,384,389,396,397,398,401,403,404,406,407,409,410,411,412,413,414,415,416,417,418,419,420,421,422,423,426,431,432,448,451,452,453,454,455,456,457,458,459,460,461,462,463,464,465,466,467,468,469,470,471,472,473],
                            toxic: {
                                indexes: [54,55,56,57,58,59,60,61,62,102,104,110,118,119,120,121,122,127,128,129,135,143,147,153,154,157,158,159,160,164,168,172,177,178,189,193,197,202,207,209,214,218,222,227,228,232,234,237,238,239,243,253,257,259,262,268,277,278,282,284,287,288,289,293,302,307,308,309,314,318,327,328,332,333,334,339,340,341,342,343,353,358,377,378,402,427,428],
                                clock: null
                            },
                            alien: {
                                current: 0,
                                path: [164,189,214,239,238,237,262,287,288,289,314,339,340,341,342,343,318,293,268,243,218,193,168,143,118,119,120,121,122,147,172,197,222],
                                clock: null
                            }
                        },
                        difficulty: 'easy',
                        key: '67890',
                        unlocked: false
                    },
                    {
                        id: 4,
                        name: 'The Frozen Room',
                        background: 'ice',
                        indexes: {
                            user: null,
                            start: 5,
                            end: 6,
                            teleports: {
                                indexes: [10,40],
                                clock: null
                            },
                            electric: {
                                indexes: [23,70],
                                copy: null,
                                clock: null
                            },
                            lava: [],
                            toxic: {
                                indexes: [300,7,2],
                                clock: null
                            },
                            alien: {
                                current: 0,
                                path: [7,8,9],
                                clock: null
                            }
                        },
                        difficulty: 'medium',
                        key: '67890',
                        unlocked: false
                    }
                ]               
            }
        };
    },
    created: function(){
        // Al created di Main, definisco con un forEach tutti i valori che hanno null di default nel data dei livelli per ogni livello
        this.datas.levels.forEach((singleLevel) => {
            singleLevel.indexes.electric.copy = singleLevel.indexes.electric.indexes;
            singleLevel.indexes.user = singleLevel.indexes.start;
            singleLevel.indexes.alien.current = 0;
        });  
    }
}
</script>

<style lang="scss" scoped>
    
</style>