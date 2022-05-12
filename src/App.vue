<template>
  <div id="app">
    <MainApp />
  </div>
</template>

<script>
import MainApp from './components/MainApp.vue'

export default {
  name: 'App',
  components: {
    MainApp
  }
}
</script>

<style lang="scss">
  @import "./style/general.scss";

  html {
    background-color: #242323;
    font-family: sans-serif;
  }
</style>
