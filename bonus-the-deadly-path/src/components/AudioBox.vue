<template>
    <!-- Box audio, se l'utente preme play parte una canzone, l'autoplay non funziona in quasi nessun browser 
    quindi l'utente deve necessariamente far partire la canzone. verrà usato nella Navbar -->
    <audio controls >
      <source src="../assets/audio/Game Music.mp3" type="audio/mpeg">
      Your browser does not support the audio element.
    </audio>
</template>

<script>
export default {
    name: 'AudioBox'
}
</script>

<style lang="scss" scoped>

</style>