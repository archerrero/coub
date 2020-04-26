<template>
  <div id="app">

    <Settings 
      @partChange="partsLength = $event"
      @colorChange="colors = $event"
      @gameChange="game = $event "
        />

    <PartsWrapper
      :colors="colors"
      :game=game
      :partsLength="Number(partsLength)" />

      

  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

import PartsWrapper from './components/PartsWrapper.vue'
import Settings from './components/Settings.vue'

export default {
  name: 'App',
  components: {
    PartsWrapper,
    Settings
  },
  data: function() {
    return {
      colors: ['#ff0000', '#0000ff'],
      inputCount: 2,
      partsLength: 3,
      game: false
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
}
</style>
