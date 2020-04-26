<template>
  <div
    @mouseenter="onHover"
    @mouseout="onHoverOut"
    @click="onClick"
    class="arrow"
    :data-row=dataRow
    :data-col=dataCol
    :class="[isActive, className]"
    :style="style"
  >
    >
  </div>
</template>

<script>
export default {
  name: "Arrow",
  props: {
    direction: {
      type: String,
      default: 'right'
    },
    part: Number,
    size: Number,
    activeClass: String
  },
  computed: {
    deg: function() {
      if (this.direction == 'top') return 'rotate(-90deg)';
      if (this.direction == 'bottom') return 'rotate(90deg)';
      if (this.direction == 'left') return 'rotate(180deg)';
      if (this.direction == 'right') return 'rotate(0)'
      return null;
    },
    dataRow: function() {
      if (this.direction == 'left' || this.direction == 'right') return this.part;
      return null;
    },
    dataCol: function() {
      if (this.direction == 'top' || this.direction == 'bottom') return this.part;
      return null;
    },
    position: function() {
      if (this.direction == 'top' || this.direction == 'bottom') 
        return {left: (this.part + 1) * this.size + 'px'};
        
      if (this.direction == 'left' || this.direction == 'right') 
        return {top: (this.part + 1) * this.size + 'px'};
      return null;
    },
    style: function() {
      return {
        transform: this.deg,
        width: this.size + 'px',
        height: this.size + 'px',
        ...this.position
      }
    },
    className: function() {
      if (this.direction == 'top' || this.direction == 'bottom') 
        return 'col_' + this.dataCol;
      if (this.direction == 'left' || this.direction == 'right') 
        return 'row_' + this.dataRow;
      return null;
    },
    isActive: function() {
      return this.activeClass == this.className ? 'active': ''
    }
  },
  methods: {
    onHover() {
      this.$emit('hover', this.className)
    },
    onHoverOut() {
      this.$emit('hover', '')
    },
    onClick() {
      this.$emit('click', {row: this.dataRow, col: this.dataCol, direction: this.direction })
    }
  }
}
</script>

<style>
  .arrow {
    font-size: 50px;
    font-family: monospace;
    line-height: 50px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    opacity: .2;
    transition: all .3s;
  }
  .float_arrow {
    position: absolute;

  }

  .active {
    opacity: 1;
    
  }


</style>