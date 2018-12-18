<template>
  <div class="view-shell">
    <div class="view-container" :id="containerID">

    </div>
    <wireframe :objects="objects" :axis="axis" :scale="axisData.scale"></wireframe>
  </div>
</template>
<script>
import {ObjectTypes} from './ObjectTypes.js'
import {AxisTypes} from './AxisTypes.js'
import SingleAxisWireFrame from './SingleAxisWireFrame.vue'
export default {
  props: ['objects', 'perspective', 'axis'],
  components: {
    wireframe: SingleAxisWireFrame
  },
  data () {
    return {
      containerID: 'el_' + Math.random().toString().split('.').join('') + Math.random().toString().split('.').join('') + Math.random().toString().split('.').join(''),
      THREE: require('three'),
      renderer: {},
      scene: {},
      camera: {},
      ObjectTypes: ObjectTypes,
      axisData: {
        axis: this.axis,
        scale: 8
      }
    }
  },
  methods: {
    whatTheFuck: function () {
      console.log('wtf')
    },
    renderObjects: function () {
      var self = this
      var THREE = self.$data.THREE
      // var renderer = self.$data.renderer
      // var camera = self.$data.camera
      var scene = self.$data.scene
      for (var i = 0; i < self.objects.length; i++) {
        var obj = {}
        var geometry = {}
        var material = {}
        var wireframe = {}
        var line = {}
        var mesh = {}
        switch (self.objects[i].type) {
          case ObjectTypes.PLANE: {
            geometry = new THREE.PlaneBufferGeometry(self.objects[i].size.width, self.objects[i].size.height, 32)
            material = new THREE.MeshBasicMaterial({side: THREE.DoubleSide, color: 0xcc0000, transparent: true})
            if (self.isOrthographic) {
              wireframe = new THREE.EdgesGeometry(geometry)
              line = new THREE.LineSegments(wireframe, material)
              line.material.depthTest = false
              line.material.opacity = 1
              line.material.transparent = true
              scene.add(line)
              obj = line
            } else {
              mesh = new THREE.Mesh(geometry, material)
              scene.add(mesh)
              obj = mesh
            }
            break
          }
          case ObjectTypes.BOX: {
            geometry = new THREE.BoxBufferGeometry(self.objects[i].size.width, self.objects[i].size.height, self.objects[i].size.depth, 32, 32, 32)
            material = new THREE.MeshBasicMaterial({color: 0xcc0000})
            if (self.isOrthographic) {
              wireframe = new THREE.EdgesGeometry(geometry)
              line = new THREE.LineSegments(wireframe, material)
              line.material.depthTest = false
              line.material.opacity = 1
              line.material.transparent = true
              scene.add(line)
              obj = line
            } else {
              mesh = new THREE.Mesh(geometry, material)
              scene.add(mesh)
              obj = mesh
            }
            break
          }
          case ObjectTypes.SPHERE: {
            geometry = new THREE.SphereBufferGeometry(self.objects[i].size.radius)
            material = new THREE.MeshBasicMaterial({color: 0xcc0000})
            if (self.isOrthographic) {
              wireframe = new THREE.EdgesGeometry(geometry)
              line = new THREE.LineSegments(wireframe, material)
              line.material.depthTest = false
              line.material.opacity = 1
              line.material.transparent = true
              scene.add(line)
              obj = line
            } else {
              mesh = new THREE.Mesh(geometry, material)
              scene.add(mesh)
              obj = mesh
            }
            break
          }
        }
        if (obj !== {}) {
          obj.position.x = self.objects[i].position.x
          obj.position.y = self.objects[i].position.y
          obj.position.z = self.objects[i].position.z
          obj.rotation.x = self.objects[i].rotation.x * (Math.PI / 180)
          obj.rotation.y = self.objects[i].rotation.y * (Math.PI / 180)
          obj.rotation.z = self.objects[i].rotation.z * (Math.PI / 180)
          obj.name = 'yo'
        }
      }
    }
  },
  computed: {
    isOrthographic: function () {
      var self = this
      return self.perspective === 0
    }
  },
  mounted: function () {
    var self = this
    // var THREE = require('three')
    var THREE = self.$data.THREE
    var containerElement = document.getElementById(self.$data.containerID)
    var _width = containerElement.getBoundingClientRect().width
    var _height = containerElement.getBoundingClientRect().height
    var renderer = new THREE.WebGLRenderer({ alpha: true })
    self.$data.renderer = renderer
    // self.$data.renderer = new self.$data.THREE.WebGLRenderer()
    renderer.setSize(_width, _height)
    containerElement.appendChild(renderer.domElement)
    var scene = new THREE.Scene()
    var camera = !self.isOrthographic > 0 ? new THREE.PerspectiveCamera(self.perspective, _width / _height, 1, 1000) : new THREE.OrthographicCamera(_width / (self.$data.axisData.scale * -2), _width / (self.$data.axisData.scale * 2), _height / (self.$data.axisData.scale * -2), _height / (self.$data.axisData.scale * 2), 1, 1000)
    if (self.isOrthographic) {
      switch (self.$data.axisData.axis) {
        case AxisTypes.X: {
          console.log('x view')
          camera.rotation.y = -90 * (Math.PI / 180)
          camera.position.x = -10
          break
        }
        case AxisTypes.Y: {
          console.log('y view')
          camera.rotation.x = 90 * (Math.PI / 180)
          camera.position.y = -10
          break
        }
        case AxisTypes.Z: {
          console.log('z view')
          // camera.rotation.z = 90 * (Math.PI / 180)
          camera.position.z = -10
          break
        }
      }
    }
    self.$data.scene = scene
    self.$data.camera = camera
    self.renderObjects()
    // var geometry = new THREE.PlaneGeometry(3, 3, 32)
    // var material = new THREE.MeshBasicMaterial({side: THREE.DoubleSide, color: 0xcc0000})
    // var plane = new THREE.Mesh(geometry, material)
    // plane.position.y = 0
    // plane.position.z = -20
    // plane.rotation.y = -45 * (Math.PI / 180)
    // plane.name = 'yo'
    // scene.add(plane)
    // material.map = new THREE.TextureLoader().load(texturePath)
    renderer.render(scene, camera)
  }
}
</script>
<style lang="scss" scoped>

div.view-shell{
  width:100%;
  height:100%;
  position: absolute;
  top: 0;
  left: 0;
  div.view-container{
    width:100%;
    height:100%;
    position: absolute;
    top: 0;
    left: 0;
  }
}
</style>
