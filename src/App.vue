<template>
  <div id="app">
    <Calculator msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import Calculator from './components/Calculator.vue';

export default {
  name: 'app',
  components: {
    Calculator
  }
};
</script>

<style lang="scss">
html {
  background-image: url('./assets/sparkly_background.png');
  height: 100%;
  background-attachment: fixed;
  background-position: center bottom;
}
</style>
