<template>
  <div class="chat-container">
    <div class="chat-header">
      <button @click="showChat">Chat</button>
      <button @click="showStickman">Image</button>
    </div>
    <ul v-show="chatVisible" class="chat-messages">
      <li v-for="message in messages" :key="message">{{ message }}</li>
    </ul>
    <form @submit.prevent="sendMessage" class="chat-form">
      <input v-model="input" autocomplete="off" />
      <button>Send</button>
    </form>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import io from 'socket.io-client';

export default {
  name: 'ChatComponent',
  setup() {
    const messages = ref([]);
    const input = ref('');
    const chatVisible = ref(true);
    const socket = io();

    const showChat = () => {
      chatVisible.value = true;
    };

    const showStickman = () => {
      chatVisible.value = false;
    };

    const sendMessage = () => {
      if (input.value.trim() !== '') {
        socket.emit('chat message', input.value);
        messages.value.push(input.value);
        input.value = '';
      }
    };

    const receiveMessage = (msg) => {
      messages.value.push(msg);
    };

    onMounted(() => {
      socket.on('chat message', (msg) => {
        receiveMessage(msg);
      });
    });

    return {
      messages,
      input,
      chatVisible,
      showChat,
      showStickman,
      sendMessage
    };
  }
};
</script>

<style scoped>
.chat-container {
  width: 50%;
  max-width: 600px;
  border: 1px solid #ccc;
  border-radius: 10px;
  overflow: hidden;
  background-color: white;
  display: flex;
  flex-direction: column;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.chat-header {
  display: flex;
  justify-content: space-between;
  padding: 10px;
  border-bottom: 1px solid #ccc;
  background-color: #007bff;
  color: white;
}

.chat-header button {
  background: none;
  border: none;
  color: white;
  font-size: 16px;
  cursor: pointer;
}

.chat-messages {
  flex: 1;
  padding: 10px;
  overflow-y: auto;
  list-style: none;
}

.chat-messages li {
  padding: 8px;
  margin-bottom: 10px;
  background-color: #f2f2f2;
  border-radius: 5px;
}

.chat-form {
  display: flex;
  border-top: 1px solid #ccc;
}

.chat-form input {
  flex: 1;
  padding: 10px;
  border: none;
  border-top: 1px solid #ccc;
}

.chat-form button {
  padding: 10px;
  background-color: #007bff;
  color: white;
  border: none;
  cursor: pointer;
}
</style>
