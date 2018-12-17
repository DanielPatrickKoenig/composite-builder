<template>
  <svg class="single-axis">
    <axiselement v-for="(o, i) in objects" :key="'group'+i.toString()" :axis='axis' :object="o" :offset="offset"></axiselement>
  </svg>
</template>
<script>
// import {AxisTypes} from './AxisTypes.js'
import SingleAxisWireFrameElement from './SingleAxisWireFrameElement.vue'
export default {
  components: {
    axiselement: SingleAxisWireFrameElement
  },
  props: ['objects', 'axis'],
  data () {
    return {
      offset: {x: 200, y: 200}
    }
  },
  mounted: function () {
    // var self = this
  }
}
</script>
<style lang="scss" scoped>
svg.single-axis{
  width:400px;
  height:400px;
}
</style>
