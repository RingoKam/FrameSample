<script setup lang="ts">
import { TresCanvas } from '@tresjs/core'
import { ref } from 'vue'
import { fpsControls, MobileJoystick  } from './fpscontrols'
import OverlayUI from './OverlayUI.vue'

const isControl = ref(false)

</script>

<template>
  <OverlayUI 
    v-model:isControl="isControl" 
  />
  <TresCanvas>
    <TresPerspectiveCamera
      :position="[0, 0, 3]"
      :fov="45"
      :aspect="1"
      :near="0.1"
      :far="1000"
    />
    <fpsControls>
    </fpsControls>
    <TresMesh>
      <TresTorusGeometry :args="[1, 0.5, 16, 32]" />
      <TresMeshBasicMaterial color="orange" />
    </TresMesh>

  </TresCanvas>
</template>

<style scoped>
</style>
