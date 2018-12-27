<template>
  <table>
    <tr class="property-header">
      <td colspan="2">
        Position
      </td>
    </tr>
    <tr>
      <td>X</td><td><input type="number" v-model="object.position.x" v-on:change="onInput" /></td>
    </tr>
    <tr>
      <td>Y</td><td><input type="number" v-model="object.position.y" v-on:change="onInput" /></td>
    </tr>
    <tr>
      <td>Z</td><td><input type="number" v-model="object.position.z" v-on:change="onInput" /></td>
    </tr>
    <tr class="property-header">
      <td colspan="2">
        Rotation
      </td>
    </tr>
    <tr>
      <td>X</td><td><input type="number" v-model="object.rotation.x" v-on:change="onInput" /></td>
    </tr>
    <tr>
      <td>Y</td><td><input type="number" v-model="object.rotation.y" v-on:change="onInput" /></td>
    </tr>
    <tr>
      <td>Z</td><td><input type="number" v-model="object.rotation.z" v-on:change="onInput" /></td>
    </tr>
    <tr class="property-header">
      <td colspan="2">
        Scale
      </td>
    </tr>
    <tr>
      <td>X</td><td><input type="number" v-model="object.scale.x" v-on:change="onInput" /></td>
    </tr>
    <tr>
      <td>Y</td><td><input type="number" v-model="object.scale.y" v-on:change="onInput" /></td>
    </tr>
    <tr>
      <td>Z</td><td><input type="number" v-model="object.scale.z" v-on:change="onInput" /></td>
    </tr>
    <tr class="property-header" v-if="object.ratio != undefined">
      <td>Ratio</td><td><input type="number" v-model="object.ratio" redraw="yes" v-on:change="onInput" /></td>
    </tr>
  </table>
</template>
<script>
import {EventBus} from './EventBus.js'
export default {
  props: ['object'],
  data () {
    return {
    }
  },
  methods: {
    onInput: function (e) {
      var shouldRedraw = e.currentTarget.getAttribute('redraw') === 'yes'
      EventBus.$emit('editor-value-change', shouldRedraw)
    },
    hasProperty: function (p) {
      return p !== null && p !== undefined
    }
  }
}
</script>

<style lang="scss" scoped>
table{
  width:100%;
  tr.property-header {
    font-weight: bold;
  }
  tr:not(.property-header){
    > td{
      width:50%;
      > input[type="number"]{
        width:100%;
      }
    }
  }
}
</style>
