<template>
  <div>
    <threerender v-if="objectManifest != null && objectManifest != undefined" :objects="objectManifest" :perspective='75'></threerender>
    <wireframe v-if="objectManifest != null && objectManifest != undefined" :objects="objectManifest" :axis="AxisTypes.X"></wireframe>
    <wireframe v-if="objectManifest != null && objectManifest != undefined" :objects="objectManifest" :axis="AxisTypes.Y"></wireframe>
    <wireframe v-if="objectManifest != null && objectManifest != undefined" :objects="objectManifest" :axis="AxisTypes.Z"></wireframe>
  </div>
</template>

<script>
import ThreeRendering from './components/ThreeRendering.vue'
import SingleAxisWireFrame from './components/SingleAxisWireFrame.vue'
import {ObjectTypes} from './components/ObjectTypes.js'
import {AxisTypes} from './components/AxisTypes.js'
export default {
  components: {
    threerender: ThreeRendering,
    wireframe: SingleAxisWireFrame
  },
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      ObjectTypes: ObjectTypes,
      AxisTypes: AxisTypes,
      objectManifest: [
        {type: ObjectTypes.PLANE, position: {x: 2, y: 5, z: -26}, rotation: {x: 0, y: 22, z: 0}, size: {width: 6, height: 5, depth: 0}},
        {type: ObjectTypes.PLANE, position: {x: 10, y: -10, z: -16}, rotation: {x: 0, y: -50, z: 0}, size: {width: 4, height: 4, depth: 0}}
      ]
    }
  }
}
</script>

<style lang="scss">
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
