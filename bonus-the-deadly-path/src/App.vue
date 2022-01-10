<template>
  <div id="app" class="w-100 vh-100">
    <!-- Stampo WebSite se il valore di day e time è diverso da null e quindi hanno assunto un valore perché dayjs ha inviato i dati -->
    <WebSite v-if="dayTimeData.currentDay !== null && dayTimeData.currentTime !== null" :dayTime="dayTimeData" />
    <!-- Stampo il Loader finché dayjs non ha caricato -->
    <Loader v-else class="w-100 vh-100" />  
  </div>
</template>

<script>
// Importo la libreria dayjs, axios e i componenti Website e Loader
import dayjs from 'dayjs';
import WebSite from './components/WebSite.vue';
import Loader from './components/Loader.vue';

export default {
  name: "App",
  components: {
   WebSite,
   Loader
  },
  data: function(){
    return {
      // Oggetto che contiene le informazioni sul giorno e ora attuale
      dayTimeData: {
      // Stringa che conterrà l'ora attuale
      currentDay: null,
      // Stringa che conterrà il giorno attuale
      currentTime: null,
      // Variabile per ripetere ogni secondo la funzione dayjs al created
      dayTimeClock: null
      }
    };
  },
  created: function(){
    // Funzione che al created si ripete ogni secondo aggiornando l'ora e data attuale
    this.dayTimeData.dayTimeClock = setInterval(() => {
      this.dayTimeData.currentDay = dayjs().format('DD/MM/YYYY');
      this.dayTimeData.currentTime = dayjs().format('HH:mm:ss');
    }, 1000); 
  }
};
</script>

<style lang="scss">
/* Import delle sass general, size-space e utilities */
@import './style/variables.scss';
@import './style/mixins.scss';
@import './style/general.scss';
@import './style/size-space.scss';
@import './style/utilities.scss';

  /* Modifiche custom per l'app */
  #app{
    background-color: $secondary_color;
    overflow-y: auto;

    /* Modifiche header */
    header {
        background-color: $secondary_color;

        > *{
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .decoration{
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }
    }
    /* Modifiche main */
    main{
        padding-top: calc($header_height + $north_south_padding);
        padding-bottom: $north_south_padding;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        gap: $between_spacing;

        /* Sezioni del main */
        section{
            @include custom_section;

            /* Titolo sezioni */
            h2{
                text-transform: uppercase;
                font-size: 40px;
                font-weight: bold;
                text-align: center;
            }
            /* Contenitore bottoni sezioni */
            .buttons_container{
                display: flex;
                flex-direction: column;
                justify-content: center;
                align-items: center;
                gap: 1rem;
            }
        }
    }
  }
</style>