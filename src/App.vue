<template>
  <div id="app" class="w-100 h-100 bg-dark">
    <Header />
    <Main />
  </div>
</template>

<script>
import Header from './components/Header.vue';
import Main from './components/Main.vue';

export default {
  name: "App",
  components: {
    Header,
    Main
  },
};
</script>

<style lang="scss">
@import './style/variables.scss';
@import './style/mixins.scss';
@import './style/general.scss';
@import './style/size-space.scss';
@import './style/utilities.scss';

</style>
