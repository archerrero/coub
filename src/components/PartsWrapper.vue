<template>
  <div class="main">
      <div
        class="wrapper"
        :style="{ 
          width: size * partsLength + 20 + 'px',
          height: size * partsLength + 20 +'px',
        }">

        <ArrowsWrapper
          :partsLength='partsLength'
          :size="size"
          @arrowClick="partMover"
        />
        <div 
          :style="{
            'background-image': backgroundImg,
            padding: '10px'
          }">

          <transition-group 
            name="flip-list"
            class="container"
            >
            <Part 
              v-for="(item, index) in partArray" 
              :key="item"
              @drag="partMover($event, index)"
              :background="backgroundImg"
              :size=size
              :partsLength="partsLength"
              :backgroundSize="size * partsLength"
              :count="item"/>
          </transition-group>

        </div>
      </div>
  </div>
</template>

<script>
import Part from './Part.vue'
import ArrowsWrapper from './ArrowsWrapper.vue'

export default {
  name: 'PartsWrapper',
  components: {
    Part,
    ArrowsWrapper
  },
  props: {
    partsLength: Number,
    colors: Array,
    game: Boolean
  },
  data: function() {
    return {
      msg: 'hello',
      size: 100,
      partArray: Array.from(Array(this.partsLength * this.partsLength).keys())
    }
  },
  beforeUpdate: function() {
    console.log(this.game)
  },
  computed: {
      backgroundImg: function() {
        let splittedColor = this.colors.join(',')

        return 'linear-gradient(135deg, ' + splittedColor + ')'
      }
  },
  watch: {
    partsLength: function () {
      this.partArray = Array.from(Array(this.partsLength * this.partsLength).keys())
    },
    game: function(val) {
      if (val){
        this.partArray = this.partArray.sort(() => Math.random() - 0.5)
        console.log('111');
        
      } else {
        this.partArray = this.partArray.sort() 
        console.log('222');
        
      }
    }
  },
  methods: {
    shuffle() {
      this.partArray = this.partArray.sort(() => Math.random() - 0.5);
    },
    partMover(e, index) {
      if(index) {

        if(e.direction == 'left' || e.direction == 'right') {
          e.row = Math.floor(index / this.partsLength)
        }

        if (e.direction == 'top' || e.direction == 'bottom') {
          e.col = index % this.partsLength
        }

      }
      if(e.row !== null) this.getRow(e.row, e.direction)
      if(e.col !== null) this.getCol(e.col, e.direction)
    },
    getRow(start, direction) {
      let from, to;
      from = start * this.partsLength
      to = (this.partsLength) * (start + 1);
      
      let temp = this.partArray.slice(from, to);

      if (direction == 'right') {
        temp.unshift(temp[temp.length - 1])
        temp.pop()
      } else {
        temp.push(temp[0])
        temp.shift()
      }

      this.partArray.splice(from, this.partsLength, ...temp);
    },
    getCol(start, direction) {
      let copy = []
      let partsLength = this.partsLength
      let changer = []

      this.partArray.forEach((item, index)=> {
        if(index % partsLength == start) {
          copy.push(item)
        } else {
          copy.push(undefined)
        }
      });
      
      if (direction == 'top') {
        changer.push(...copy.slice(partsLength))
        changer.push(...copy.slice(0, partsLength ))
      } else {
        changer.push(...copy.slice(copy.length - partsLength))
        changer.push(...copy.slice(0, copy.length - partsLength))
      }

      this.partArray = this.partArray.map((item, index) => {
        if (changer[index] !== undefined) {
          return changer[index]
        }
        return item
      });
    }
  }
}
</script>

<style>
.wrapper {
  /* display: flex;
  flex-wrap: wrap; */
  margin: auto;
  position: relative;
  padding: 100px;
}
.container {
  display: flex;
  flex-wrap: wrap;
  /* background: #fff; */
}
.flip-list-move {
  transition: transform 1s;
}
.main {
}


</style>