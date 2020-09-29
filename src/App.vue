<template>
  <div id="app">
    <BuscaProdutos/>
  </div>
</template>

<script>
import BuscaProdutos from './components/BuscaProdutos.vue';

export default {
  name: 'App',
  components: {
    BuscaProdutos,
  },
};
</script>

<style lang="scss">
  @import './assets/scss/app.scss';
</style>
