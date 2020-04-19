<template>
  <div id="app">
    <game></game>
  </div>
</template>

<script>
import Game from './components/Game';
export default {
  name: 'app',
  components: {
    Game
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<style lang="scss">
*{
  box-sizing: border-box;
}
</style>
