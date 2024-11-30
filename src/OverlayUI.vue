<template>
    <!-- 
    TODO: hide and show depending on devices
    example, if mobile, show input box
    if desktop, show id
    
    TODO: figure out what we need to send in between mobile and desktop
    -->
  <div class="overlay-ui">
    HELLO I AM HERE!
    <h1>{{ myId }}</h1>
    <input v-model="inputId" @keydown.enter="onConnect"/>
    <button @click="onConnect" >Connect</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const peer = new Peer();
const myId = ref();
const inputId = ref("");

peer.on('open', (id) => {
    console.log('My peer ID is: ' + id);
    myId.value = id;
});

peer.on('connection', function(conn) { 
    alert("you are connected!");
});

const onConnect = () => {
    console.log("connecting to " + inputId.value);
    const conn = peer.connect(inputId.value);
}    

</script>

<style scoped>
.overlay-ui {
    position: absolute;
    z-index: 100;
}
</style>