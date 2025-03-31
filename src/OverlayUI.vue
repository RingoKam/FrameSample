<template>
    <!-- 
    TODO: Get Gyro scope working
    TODO: Start building out the parent UI
    TODO: Start designing the environment
    -->
  <div class="overlay-ui">
    HELLO I AM HERE!
    <h1>{{ myId }}</h1>
    <input v-model="inputId" @keydown.enter="onConnect"/>
    <button @click="onConnect" >Connect</button>
  </div>
</template>

<script setup>
import { ref, defineProps, defineEmits } from 'vue'

const props = defineProps({
    isControl: Boolean,
    camera: Object
})

const emits = defineEmits(['update:isControl'])

const peer = new Peer();
const myId = ref();

const inputId = ref("");

setInterval(() => {
    var cameraJson = camera.toJSON("matrix")
    console.log(cameraJson)
}, 1000)

peer.on('open', (id) => {
    console.log('My peer ID is: ' + id);
    myId.value = id;
});

// screen connected
peer.on('connection', function(conn) { 
    alert("you are connected!");
});

const onConnect = () => {
    console.log("connecting to " + inputId.value);
    const conn = peer.connect(inputId.value);
    console.log(conn);
    // on connected, set isControl to true
    conn.on('open', () => {
        console.log("connected to " + inputId.value);
        emits('update:isControl', true);
    });
}    

</script>

<style scoped>
.overlay-ui {
    position: absolute;
    z-index: 100;
}
</style>