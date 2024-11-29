<script setup lang="ts">
import { TresCanvas } from '@tresjs/core'
import { fpsControls  } from './fpscontrols'

</script>

<template>
  <TresCanvas>
    <TresPerspectiveCamera
      :position="[0, 0, 3]"
      :fov="45"
      :aspect="1"
      :near="0.1"
      :far="1000"
    />
    <fpsControls />
    <TresMesh>
      <TresTorusGeometry :args="[1, 0.5, 16, 32]" />
      <TresMeshBasicMaterial color="orange" />
    </TresMesh>
  </TresCanvas>
</template>

<style scoped>
</style>
