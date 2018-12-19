<template>
  <div :id="containerID" class="centered-element" v-if="proxies.x != null && proxies.x != undefined">
    <a v-on:mousedown="onDown" :style="'left:'+((object.position[proxies.x]*scale)).toString()+'px;top:'+((object.position[proxies.y]*scale)).toString()+'px;width:'+moverSize.width.toString()+'px;height:'+moverSize.height.toString()+'px;margin-left:'+(moverSize.width/-2).toString()+'px;margin-top:'+(moverSize.height/-2).toString()+'px;'"></a>
    <div v-if="dragging" v-on:mousemove="onMove" v-on:mouseup="onUp" class="element-drag-layer"></div>
  </div>
</template>
<script>
import {EventBus} from './EventBus.js'
import {AxisTypes} from './AxisTypes.js'
export default {
  props: ['axis', 'object', 'offset', 'scale', 'index'],
  data () {
    return {
      containerID: 'elementWireFrameID_' + Math.random().toString().split('.').join('') + Math.random().toString().split('.').join('') + Math.random().toString().split('.').join(''),
      AxisTypes: AxisTypes,
      proxies: {},
      moverSize: {width: 12, height: 12},
      dragging: false
    }
  },
  methods: {
    onDown: function (e) {
      var self = this
      self.$data.dragging = true
    },
    onMove: function (e) {
      var self = this
      EventBus.$emit('wireframe-drag-element', {
        axis: self.axis,
        index: self.index,
        x: e.pageX - document.getElementById(self.$data.containerID).getBoundingClientRect().left,
        y: e.pageY - document.getElementById(self.$data.containerID).getBoundingClientRect().top,
        scale: self.scale
      })
    },
    onUp: function (e) {
      var self = this
      self.$data.dragging = false
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
  > div.element-drag-layer{
    position: fixed;
    top:0;
    left:0;
    right:0;
    bottom:0;
    z-index:400;
  }
}

</style>


