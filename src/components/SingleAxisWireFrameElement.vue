<template>
  <g v-if="proxies.x != null && proxies.x != undefined">
    <circle :cx="object.position[proxies.x] + offset.x"  :cy="object.position[proxies.y] + offset.x" r="4" />
  </g>
</template>
<script>
import {AxisTypes} from './AxisTypes.js'
export default {
  props: ['axis', 'object', 'offset'],
  data () {
    return {
      AxisTypes: AxisTypes,
      proxies: {}
    }
  },
  mounted: function () {
    var self = this
    switch (self.axis) {
      case AxisTypes.X: {
        self.$data.proxies = {x: 'z', y: 'y'}
        break
      }
      case AxisTypes.Y: {
        self.$data.proxies = {x: 'x', y: 'z'}
        break
      }
      case AxisTypes.Z: {
        self.$data.proxies = {x: 'x', y: 'y'}
        break
      }
    }
  }
}
</script>

