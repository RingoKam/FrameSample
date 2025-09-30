<script setup lang="ts">
import { TresCanvas } from '@tresjs/core'
import { Matrix4 , Vector3, Quaternion } from 'three'; 
import { ref, useTemplateRef } from 'vue'
import { fpsControls, MobileJoystick  } from './fpscontrols'
import OverlayUI from './OverlayUI.vue'

const isControl = ref(false)
const cameraPosition = ref([0, 0, 3])
const isMobileControl = ref(navigator.maxTouchPoints > 1)
const camera = useTemplateRef("camera")

const syncCamera = (matrix) => {
    var cameraMatrix = new Matrix4().fromArray(matrix);

    const position = new Vector3();
    const quaternion = new Quaternion();
    const scale = new Vector3();
    
    cameraMatrix.decompose(position, quaternion, scale);

    console.log("Position:", position);
    console.log("Rotation (Quaternion):", quaternion);
    console.log("Scale:", scale);
    
    // TODO: hookup gyro scope next! 
    cameraPosition.value = position
} 

</script>

<template>
  <OverlayUI 
    v-model:isControl="isControl"
    :camera="camera"
    @sync-camera="syncCamera"
  />
  <TresCanvas>
    <!-- track the camera position and emit that out as an event 
    to mobile so we can sync up both camera-->
    <TresPerspectiveCamera
      ref="camera"
      :position="cameraPosition"
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
