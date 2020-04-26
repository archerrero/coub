<template>
  <div >
    <div
      v-for="(part, index) in partsLength"
      :key="index">

      <Arrow
        v-for="(item) in arrowConf"
        :key="item"
        :part='index'
        :size="size"
        :class="['arrow_' + item, 'float_arrow']"
        :direction="item"
        :activeClass=activeClass
        @hover="onHover"
        @click="onClick"
      />
    </div>
  </div>
</template>

<script>
import Arrow from './Arrow.vue';

export default {
  name: "ArrowsWrapper",
  components: {
    Arrow
  },
  props: {
    partsLength: Number,
    part: Number,
    size: Number,
  },
  data: function() {
    return {
      arrowConf: [
        'top',
        'bottom',
        'left',
        'right',
      ],
      activeClass: ''
    }
  },
  methods: {
    onHover(e) {
      this.activeClass = e
    },
    onClick(e) {
      this.$emit('arrowClick', e)
    }
  }
}
</script>

<style>
  .arrow_top {
  top: 0;
}
.arrow_bottom {
  bottom: 0;
}
.arrow_left {
  left: 0;
}
.arrow_right {
  right: 0;
}
</style>