<template>
  <div class="centered-element" v-if="proxies.x != null && proxies.x != undefined">
    <a :style="'left:'+((object.position[proxies.x]*scale)).toString()+'px;top:'+((object.position[proxies.y]*scale)).toString()+'px;width:'+moverSize.width.toString()+'px;height:'+moverSize.height.toString()+'px;margin-left:'+(moverSize.width/-2).toString()+'px;margin-top:'+(moverSize.height/-2).toString()+'px;'"></a>
  </div>
</template>
<script>
import {AxisTypes} from './AxisTypes.js'
export default {
  props: ['axis', 'object', 'offset', 'scale'],
  data () {
    return {
      AxisTypes: AxisTypes,
      proxies: {},
      moverSize: {width: 12, height: 12}
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
<style lang="scss" scoped>
div.centered-element{
  left:50%;
  top:50%;
  position: absolute;;
  width:0;
  height: 0;
  > a{
    position: absolute;
    background-color: #000000;
    border-radius: 200px;
  }
}
</style>


