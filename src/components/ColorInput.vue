<template>
  <label>
    <div>Color</div>
    <input 
      type="color" 
      name="" 
      :value="value"
      @input="$emit('input', $event.target.value)" >
    <!-- <input type="color" name="" id="" @change="setColor($event)" v-model="colors[0]"> -->
  </label>
</template>

<script>
export default {
  name: 'ColorInput',
  props: {
  },
  data: function() {
    return {
      value: ''
    }
  }
}
</script>