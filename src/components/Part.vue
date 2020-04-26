<template>
    <div
      @mousedown.prevent="onDragStart"
      @mousemove.prevent="onDragMove"
      @mouseup.prevent="onDragEnd"
      :style="{ 
        width: size + 'px',
        height: size + 'px',
        'background-image': background,
        'background-size': backgroundSize + 'px '+ backgroundSize + 'px',
        'background-position': backgroundPosition,
       }">
    </div>
</template>

<script>
export default {
  name: 'Part',
  props: {
      count: Number,
      background: String,
      size: Number,
      backgroundSize: Number,
      partsLength: Number,
  },
  data: function() {
    return {
      dragData: {},
      teimer: () => null,
    }
  },
  computed: {
    row: function() {
      return Math.floor(this.count / this.partsLength)
    },
    col: function() {
      return this.count % this.partsLength
    },
    backgroundPosition: function() {
      let x = -this.col * this.size
      let y = -this.row * this.size

      return x + 'px ' + y + 'px'
    }
  },
  methods: {
    onDragStart(e) {
      console.log(e)
      Object.assign(this.dragData, {
          isPressed: true,
          x: e.x, 
          y: e.y
        })
    },
    onDragEnd() {
      Object.assign(this.dragData, {
          isPressed: false,
          x: null, 
          y: null
        })
    },
    onDragMove(e) {
      if (this.dragData.isPressed) {
        clearTimeout(this.teimer);

        let x = e.x - this.dragData.x;
        let y = e.y - this.dragData.y;
        let data = {};
        

        this.teimer = setTimeout(() => {

          if (Math.abs(x) > Math.abs(y)) {
            // row
            data = {
              direction: (x > 0) ? 'right' : 'left',
              row: this.row,
              col: null
            }
          } else {
            // col
            data = {
              direction:  (y > 0) ? 'bottom' : 'top',
              col: this.col,
              row: null
            }
          }

          this.$emit('drag', data)
          Object.assign(this.dragData, {
            isPressed: false,
            x: null, 
            y: null
          })
        }, 300);
        
      }
    }
  }
}
</script>