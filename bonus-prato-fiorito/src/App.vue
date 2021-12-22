<template>
  <div id="app" class="bg_emerald">
    <Header />
    <Main />
    <Footer />
  </div>
</template>

<script>
import Header from './components/Header.vue';
import Main from './components/Main.vue';
import Footer from './components/Footer.vue';

export default {
  name: "App",
  components: {
   Header,
   Main,
   Footer
  },
};
</script>

<style lang="scss">
@import './style/general.scss';
@import './style/size-space.scss';
  
</style>
