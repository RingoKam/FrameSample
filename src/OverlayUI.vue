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

const emits = defineEmits(['update:isControl', 'sync-camera'])

const peer = new Peer();
const myId = ref();

const inputId = ref("");

peer.on('open', (id) => {
    console.log('My peer ID is: ' + id);
    myId.value = id;
});

// screen connected
peer.on('connection', function(conn) { 
    alert("you are connected!");

     conn.on('data', ({payload}) => {
        console.log("package received", payload)
        emits("sync-camera", payload.matrix)
    })
});

const onConnect = () => {
    console.log("connecting to " + inputId.value);
    const conn = peer.connect(inputId.value);

    // on connected, set isControl to true
    conn.on('open', () => {
        console.log("connected to " + inputId.value);
        emits('update:isControl', true);

        setInterval(() => {
            var cameraJson = props.camera.toJSON()
            var matrix = cameraJson.object.matrix;
            var { position, rotation } = cameraJson.object;
            console.log(matrix, position, rotation)
            conn.send({
                type: "camera:update",
                payload: {
                    matrix,
                    position
                }
            })
        }, 1000)
    });
}    

</script>

<style scoped>
.overlay-ui {
    position: absolute;
    z-index: 100;
}
</style>