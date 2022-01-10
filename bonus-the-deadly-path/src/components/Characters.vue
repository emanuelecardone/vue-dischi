<template>
    <section class="characters">
        <h2>characters</h2>
        <ul class="icons_list">
            <li v-for="icon,index in user.icons" @click="pickIcon(icon)" :key="index" :class="{'selected': current.character.icon === icon}">
                <User :class="'fs_30 fas fa-' + icon" />
            </li>
        </ul>
        <ul class="colors_list">
            <li class="color" v-for="color,index in user.colors" @click="pickColor(color)" :key="index" :class="{'selected': current.character.color === color}">
                <div class="bg_fix position-absolute rounded-circle" :class="'bg-' + color"></div>
            </li>
        </ul>
        <div class="preview">
            <h3 class="mb-3">Current:</h3>
            <User :class="'fs_30 fas fa-' + current.character.icon + ' text-' + current.character.color" />
        </div>
        <Button @click.native="leaveSection" :msg="'choose'" />
    </section>
</template>

<script>
import User from './User.vue';
import Button from './Button.vue';

export default {
    name: 'Characters',
    components: {
        User,
        Button
    },
    props: {
        status: Object,
        user: Object,
        current: Object
    },
    methods: {
        // Funzione per passare alla sezione successiva
        leaveSection: function(){
            this.status.characters = false;
            this.status.levels = true;
        },
        // Funzione per cambiare l'icona selezionata e aggiornare la classe dell'icona attuale per l'utente
        pickIcon: function(clickedIcon){
            this.current.character.icon = clickedIcon;
        },
        // Funzione per cambiare il colore selezionato e aggiornare la classe del colore attuale per l'utente
        pickColor: function(clickedColor){
            this.current.character.color = clickedColor;
        }
    }
}
</script>

<style lang="scss" scoped>
@import '../style/variables.scss';

    .characters{

        /* Modifiche ul */
        ul{
            display: flex;
            gap: 1rem;
            padding: 0;

            /* Modifiche li */
            li{
                width: 50px;
                height: 50px;
                display: flex;
                justify-content: center;
                align-items: center;
                padding: 3rem;
                border: 2px solid $secondary_color;
                border-radius: 50%;
                cursor: pointer;
                position: relative;

                &.selected{
                    animation: selected_item 1s linear infinite;
                }
                .bg_fix{
                    width: calc(100% + 2px);
                    height: calc(100% + 2px);
                }
            }
        }
        /* Modifiche box preview */
        .preview{
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }
    }

</style>