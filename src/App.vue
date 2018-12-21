<template>
  <div>
    <div class="viewports">
      <div :style="getQuatStyle(false, false)">
        <threerender class="prespectiv-view" v-if="shouldRender" :objects="objectManifest" :perspective='75' :signature="signature"></threerender>
      </div>
      <div :style="getQuatStyle(true, true)">
        <!-- <wireframe :objects="objectManifest" :axis="AxisTypes.X" v-if="shouldRender"></wireframe> -->
        <threerender class="x-view" v-if="shouldRender" :objects="objectManifest" :perspective='0' :axis="AxisTypes.X" :signature="signature"></threerender>
      </div>
      <div :style="getQuatStyle(false, true)">
        <!-- <wireframe :objects="objectManifest" :axis="AxisTypes.Y" v-if="shouldRender"></wireframe> -->
        <threerender class="y-view" v-if="shouldRender" :objects="objectManifest" :perspective='0' :axis="AxisTypes.Y" :signature="signature"></threerender>
      </div>
      <div :style="getQuatStyle(true, false)">
        <!-- <wireframe :objects="objectManifest" :axis="AxisTypes.Z" v-if="shouldRender"></wireframe> -->
        <threerender class="z-view" v-if="shouldRender" :objects="objectManifest" :perspective='0' :axis="AxisTypes.Z" :signature="signature"></threerender>
      </div>
    </div>
    <layers :layers="objectManifest" :z="2000" class="object-listing">
      <div v-for="(o, i) in objectManifest" :key="'objectlayer'+i.toString" :slot="'slot'+i.toString()">
        <label>
          {{getTypeLabel(o.type)}}
        </label>
        <editor :object="o"></editor>
      </div>
    </layers>
  </div>
</template>

<script>
import {EventBus} from './components/EventBus.js'
import ThreeRendering from './components/ThreeRendering.vue'
import LayerList from './components/LayerList.vue'
import ThreeObjectEditor from './components/ThreeObjectEditor.vue'
import {ObjectTypes} from './components/ObjectTypes.js'
import {AxisTypes} from './components/AxisTypes.js'
export default {
  components: {
    threerender: ThreeRendering,
    layers: LayerList,
    editor: ThreeObjectEditor
  },
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      ObjectTypes: ObjectTypes,
      AxisTypes: AxisTypes,
      objectManifest: [
        {type: ObjectTypes.PLANE, position: {x: 2, y: 5, z: -26}, rotation: {x: 0, y: 22, z: 0}, scale: {x: 6, y: 5, z: 1}},
        {type: ObjectTypes.PLANE, position: {x: 10, y: -10, z: -16}, rotation: {x: 0, y: -50, z: 0}, scale: {x: 4, y: 4, z: 1}},
        {type: ObjectTypes.BOX, position: {x: -1, y: 0, z: -11}, rotation: {x: 0, y: -40, z: 0}, scale: {x: 4, y: 3, z: 2}},
        {type: ObjectTypes.SPHERE, position: {x: 3, y: 2, z: -11}, rotation: {x: 0, y: -40, z: 0}, scale: {x: 4, y: 3, z: 2}}
      ],
      centers: {x: 50, y: 50},
      canRenderViewports: true,
      signature: 0
    }
  },
  computed: {
    shouldRender: function () {
      var self = this
      return self.$data.canRenderViewports && self.$data.objectManifest !== null && self.$data.objectManifest !== undefined
    }
  },
  methods: {
    getQuatStyle: function (isLeft, isTop) {
      var self = this
      var left = isLeft ? 0 : self.$data.centers.x
      var top = isTop ? 0 : self.$data.centers.y
      var width = isLeft ? self.$data.centers.x : 100 - self.$data.centers.x
      var height = isTop ? self.$data.centers.y : 100 - self.$data.centers.y
      return 'left:' + left + '%;top:' + top + '%;width:' + width + '%;height:' + height + '%;'
    },
    rerenderViewports: function () {
      var self = this
      self.$data.canRenderViewports = false
      setTimeout(function () {
        self.$data.canRenderViewports = true
      }, 10)
    },
    updateViewports: function () {
      var self = this
      self.$data.signature += 1
      if (self.$data.signature > 20) {
        self.$data.signature = 0
      }
    },
    getTypeLabel: function (t) {
      var labelTxt = ''
      switch (t) {
        case ObjectTypes.PLANE: {
          labelTxt = 'Plane'
          break
        }
        case ObjectTypes.BOX: {
          labelTxt = 'Cube'
          break
        }
        case ObjectTypes.SPHERE: {
          labelTxt = 'Sphere'
          break
        }
      }
      return labelTxt
    }
  },
  mounted: function () {
    var self = this
    window.addEventListener('resize', function () {
      self.rerenderViewports()
    })
    EventBus.$on('wireframe-drag-element', (n) => {
      console.log(n)
      switch (n.axis) {
        case AxisTypes.X: {
          self.$data.objectManifest[n.index].position.z = n.x / n.scale
          self.$data.objectManifest[n.index].position.y = (n.y / n.scale)
          break
        }
        case AxisTypes.Y: {
          self.$data.objectManifest[n.index].position.x = n.x / n.scale
          self.$data.objectManifest[n.index].position.z = n.y / n.scale
          break
        }
        case AxisTypes.Z: {
          self.$data.objectManifest[n.index].position.x = n.x / n.scale
          self.$data.objectManifest[n.index].position.y = (n.y / n.scale)
          break
        }
      }
      self.updateViewports()
    })
    EventBus.$on('wireframe-rotate-element', (n) => {
      // console.log(self.$data.objectManifest[n.index])
      switch (n.axis) {
        case AxisTypes.X: {
          // self.$data.objectManifest[n.index].rotation.set(n.r, self.$data.objectManifest[n.index].rotation.y, self.$data.objectManifest[n.index].rotation.z)
          self.$data.objectManifest[n.index].rotation.x = n.r
          break
        }
        case AxisTypes.Y: {
          // self.$data.objectManifest[n.index].rotation.set(self.$data.objectManifest[n.index].rotation.x, n.r, self.$data.objectManifest[n.index].rotation.z)
          self.$data.objectManifest[n.index].rotation.y = n.r
          break
        }
        case AxisTypes.Z: {
          // self.$data.objectManifest[n.index].rotation.set(self.$data.objectManifest[n.index].rotation.x, self.$data.objectManifest[n.index].rotation.y, n.r)
          self.$data.objectManifest[n.index].rotation.z = n.r
          break
        }
      }
      self.updateViewports()
    })
    EventBus.$on('wireframe-scale-x-element', (n) => {
      console.log(n)
      self.$data.objectManifest[n.index].scale.x = n.s
      self.updateViewports()
    })
    EventBus.$on('wireframe-scale-y-element', (n) => {
      self.$data.objectManifest[n.index].scale.y = n.s
      self.updateViewports()
    })
    EventBus.$on('wireframe-scale-z-element', (n) => {
      self.$data.objectManifest[n.index].scale.z = n.s
      self.updateViewports()
    })
    EventBus.$on('editor-value-change', (n) => {
      self.updateViewports()
    })
  }
}
</script>

<style lang="scss">
div.viewports{
  position: fixed;
  top:0;
  left:0;
  right:250px;
  bottom:0;
  > div{
    position: absolute;
    box-shadow: 0 0 0 1px #000000 inset
  }
}
.object-listing{
  width:250px !important;
  position: fixed;
  top:0;
  right:0;
  bottom:0;
  overflow-x: hidden;
  overflow-y: auto;
  padding: 0 !important;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
