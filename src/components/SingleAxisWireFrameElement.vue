<template>
  <div :id="containerID" class="centered-element" v-if="proxies.x != null && proxies.x != undefined">
    <a v-on:mousedown="onDragDown" :style="'left:'+(object.position[proxies.x]*scale).toString()+'px;top:'+(object.position[proxies.y]*scale*(yFactor)).toString()+'px;width:'+moverSize.width.toString()+'px;height:'+moverSize.height.toString()+'px;margin-left:'+(moverSize.width/-2).toString()+'px;margin-top:'+(moverSize.height/-2).toString()+'px;'"></a>
    <div v-if="dragging" v-on:mousemove="onDragMove" v-on:mouseup="onDragUp" class="element-edit-layer"></div>
    <a v-on:mousedown="onRotateDown" :style="'left:'+orbit(true).toString()+'px;top:'+orbit().toString()+'px;width:'+moverSize.width.toString()+'px;height:'+moverSize.height.toString()+'px;margin-left:'+(moverSize.width/-2).toString()+'px;margin-top:'+(moverSize.height/-2).toString()+'px;'"></a>
    <div v-if="rotating" v-on:mousemove="onRotateMove" v-on:mouseup="onRotateUp" class="element-edit-layer"></div>
  </div>
</template>
<script>
import {EventBus} from './EventBus.js'
import {AxisTypes} from './AxisTypes.js'
import {getOrbit, getAngle} from './utils/Trig.js'
export default {
  props: ['axis', 'object', 'offset', 'scale', 'index'],
  data () {
    return {
      containerID: 'elementWireFrameID_' + Math.random().toString().split('.').join('') + Math.random().toString().split('.').join('') + Math.random().toString().split('.').join(''),
      AxisTypes: AxisTypes,
      proxies: {},
      moverSize: {width: 12, height: 12},
      yFactor: this.axis === AxisTypes.Y ? 1 : -1,
      yBuffer: this.axis === AxisTypes.Y ? 0 : 180,
      dragging: false,
      rotating: false
    }
  },
  methods: {
    onDragDown: function (e) {
      var self = this
      self.$data.dragging = true
    },
    onDragMove: function (e) {
      var self = this
      EventBus.$emit('wireframe-drag-element', {
        axis: self.axis,
        index: self.index,
        x: e.pageX - document.getElementById(self.$data.containerID).getBoundingClientRect().left,
        y: self.axis === AxisTypes.Y ? e.pageY - document.getElementById(self.$data.containerID).getBoundingClientRect().top : (e.pageY - document.getElementById(self.$data.containerID).getBoundingClientRect().top) * -1,
        scale: self.scale
      })
    },
    onDragUp: function (e) {
      var self = this
      self.$data.dragging = false
    },
    onRotateDown: function (e) {
      var self = this
      self.$data.rotating = true
    },
    onRotateMove: function (e) {
      var self = this
      var xVal = self.object.position[self.$data.proxies.x] * self.scale
      var yVal = self.object.position[self.$data.proxies.y] * self.scale
      EventBus.$emit('wireframe-rotate-element', {
        axis: self.axis,
        index: self.index,
        r: (getAngle(xVal, yVal, e.pageX - document.getElementById(self.$data.containerID).getBoundingClientRect().left, (e.pageY - document.getElementById(self.$data.containerID).getBoundingClientRect().top) * self.$data.yFactor) * self.$data.yFactor) + self.$data.yBuffer,
        scale: self.scale
      })
    },
    onRotateUp: function (e) {
      var self = this
      self.$data.rotating = false
    },
    orbit: function (isX) {
      var self = this
      var rotatorDist = 30
      var val = 0
      if (isX) {
        val = getOrbit(self.object.position[self.$data.proxies.x] * self.scale, rotatorDist, self.object.rotation[self.$data.proxies.r], 'cos')
      } else {
        val = getOrbit(self.object.position[self.$data.proxies.y] * self.scale * self.$data.yFactor, rotatorDist, self.object.rotation[self.$data.proxies.r], 'sin')
      }
      return val
    }
  },
  mounted: function () {
    var self = this
    switch (self.axis) {
      case AxisTypes.X: {
        self.$data.proxies = {x: 'z', y: 'y', r: 'x'}
        break
      }
      case AxisTypes.Y: {
        self.$data.proxies = {x: 'x', y: 'z', r: 'y'}
        break
      }
      case AxisTypes.Z: {
        self.$data.proxies = {x: 'x', y: 'y', r: 'z'}
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
  > div.element-edit-layer{
    position: fixed;
    top:0;
    left:0;
    right:0;
    bottom:0;
    z-index:400;
  }
}

</style>


