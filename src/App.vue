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
  @import "./style/general.scss"
</style>
