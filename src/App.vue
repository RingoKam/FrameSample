<script setup lang="ts">
import { TresCanvas } from '@tresjs/core'
import { ref, useTemplateRef } from 'vue'
import { fpsControls, MobileJoystick  } from './fpscontrols'
import OverlayUI from './OverlayUI.vue'

const isControl = ref(false)
const isMobileControl = ref(navigator.maxTouchPoints > 1)
const camera = useTemplateRef("camera")

</script>

<template>
  <OverlayUI 
    v-model:isControl="isControl"
    :camera="camera"
  />
  <TresCanvas>
    <!-- track the camera position and emit that out as an event 
    to mobile so we can sync up both camera-->
    <TresPerspectiveCamera
      ref="camera"
      :position="[0, 0, 3]"
      :fov="45"
      :aspect="1"
      :near="0.1"
      :far="1000"
    />
    <fpsControls>
      <MobileJoystick />
    </fpsControls>
    <TresMesh>
      <TresTorusGeometry :args="[1, 0.5, 16, 32]" />
      <TresMeshBasicMaterial color="orange" />
    </TresMesh>

  </TresCanvas>
</template>

<style scoped>
</style>
