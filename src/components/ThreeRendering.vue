<template>
  <div class="view-container" :id="containerID">

  </div>
</template>
<script>
import {ObjectTypes} from './ObjectTypes.js'
export default {
  props: ['objects', 'perspective'],
  data () {
    return {
      containerID: 'el_' + Math.random().toString().split('.').join('') + Math.random().toString().split('.').join('') + Math.random().toString().split('.').join(''),
      THREE: require('three'),
      renderer: {},
      scene: {},
      camera: {},
      ObjectTypes: ObjectTypes
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
        switch (self.objects[i].type) {
          case ObjectTypes.PLANE: {
            var geometry = new THREE.PlaneGeometry(self.objects[i].size.width, self.objects[i].size.height, 32)
            var material = new THREE.MeshBasicMaterial({side: THREE.DoubleSide, color: 0xcc0000})
            var plane = new THREE.Mesh(geometry, material)
            plane.position.x = self.objects[i].position.x
            plane.position.y = self.objects[i].position.y
            plane.position.z = self.objects[i].position.z
            plane.rotation.x = self.objects[i].rotation.x * (Math.PI / 180)
            plane.rotation.y = self.objects[i].rotation.y * (Math.PI / 180)
            plane.rotation.z = self.objects[i].rotation.z * (Math.PI / 180)
            plane.name = 'yo'
            scene.add(plane)
            break
          }
          case ObjectTypes.BOX: {
            break
          }
          case ObjectTypes.SPHERE: {
            break
          }
        }
      }
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
    var camera = new THREE.PerspectiveCamera(self.perspective, _width / _height, 1, 1000)
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
div.view-container{
  width:400px;
  height:400px;
}
</style>
