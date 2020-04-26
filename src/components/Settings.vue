<template>
  <div class="settings-wrapper">
    <div
      v-show="!game"
      class="counter">
      <Arrow 
        direction="left" 
        @click="partsLength--" />

      <input 
        type="text" 
        v-model="partsLength" >

      <Arrow 
        direction="right" 
        @click="partsLength++" />
    </div>

    <div
      v-show="!game"
      class="color">
      <div>
        <ColorInput
          v-for="(input, index) in inputCount"
          :key="index"
          @input="$emit('colorChange', colors)"
          v-model="colors[index]"/>
      </div>

      <button @click="addColor">Add Color</button>
    </div>
    <button
      @click="gameChanger">
      {{game ? 'Stop!': 'Start!'}}
    </button>
  </div>
</template>

<script>

import Arrow from './Arrow.vue'
import ColorInput from './ColorInput.vue'

export default {
  name: 'Settings',
  components: {
    Arrow,
    ColorInput
  },
  props: {

  },
  data: function() {
    return {
      partsLength: 3,
      colors: ['#ff0000', '#0000ff'],
      inputCount: 2,
      game: false
    }
  },
  methods: {
    addColor() {
      this.inputCount++
    },
    partChangeeEmmiter() {
      this.$emit('partChange', this.partsLength)
    },
    gameChanger() {
      this.game = !this.game
      console.log('qwe', this.game)
      this.$emit('gameChange', this.game)
    }
  },
  watch: {
    partsLength: function(val, oldVal) {
      if(val < 2 || val > 20) this.partsLength = oldVal
      this.partChangeeEmmiter()
    }
  }
}
</script>

<style>

  .settings-wrapper {
    display: flex;
    align-items: center;
    justify-content: space-around;
  }
  .counter {
    display: flex;
  }
  .color {
    display: flex;
    align-items: center;
    justify-content: space-around;
  }
  input {
    width: 100px;
    font-size: 25px;
    text-align: center;
    margin: 0 25px;
  }
  button {
    padding: 25px;
  }
</style>