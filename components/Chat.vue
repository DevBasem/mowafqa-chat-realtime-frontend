<template>
    <div>
        <div>
            <ul>
                <li v-for="(message, index) in messages" :key="index">{{ message }}</li>
            </ul>
        </div>
        <input v-model="input" @keydown.enter="sendMessage" placeholder="Type a message" />
    </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import io from 'socket.io-client';
const socket = io('http://localhost:3001');

const messages = ref([]);
const input = ref('');

const sendMessage = () => {
    if (input.value.trim()) {
        socket.emit('chat message', input.value);
        input.value = '';
    }
};

onMounted(() => {
    socket.on('chat message', (message) => {
        messages.value.push(message);
    });
});

onUnmounted(() => {
    socket.disconnect();
});
</script>

<style scoped>

</style>