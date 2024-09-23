<template>
  <div class="chat-stickman-container" v-show="!chatVisible">
    <div class="stickman-wrapper">
      <img :src="stickmanSrc" alt="Stickman" class="chat-stickman">
      <div id="bubble-container" class="bubble-container">
        <div v-for="(bubble, index) in bubbles" :key="index" class="chat-bubble">{{ bubble }}</div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted, watch } from 'vue';

export default {
  name: 'StickmanComponent',
  setup(props, { emit }) {
    const bubbles = ref([]);
    const stickmanSrc = ref('stickman2.png');
    const chatTimeout = ref(null);

    const addBubble = (msg) => {
      bubbles.value.push(msg);
      setTimeout(() => {
        bubbles.value.shift();
      }, 3000);
    };

    const resetChatTimeout = () => {
      clearTimeout(chatTimeout.value);
      chatTimeout.value = setTimeout(() => {
        stickmanSrc.value = 'stickman2.png';
      }, 5000);
    };

    watch(
      () => props.chatVisible,
      (newVal) => {
        if (!newVal) {
          stickmanSrc.value = 'stickman.png';
          resetChatTimeout();
        }
      }
    );

    onMounted(() => {
      emit('newMessage', (msg) => {
        addBubble(msg);
      });
    });

    return {
      bubbles,
      stickmanSrc,
      resetChatTimeout
    };
  },
  props: {
    chatVisible: Boolean
  }
};
</script>

<style scoped>
.chat-stickman-container {
  display: flex;
  flex: 1;
  justify-content: center;
  align-items: center;
  padding: 20px;
  position: relative;
}

.stickman-wrapper {
  position: relative;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.chat-stickman {
  width: 20%;
  height: auto;
}

.bubble-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  pointer-events: none;
}

.chat-bubble {
  background-color: rgba(0, 0, 0, 0.7);
  color: white;
  padding: 10px;
  border-radius: 10px;
  max-width: 80%;
  word-wrap: break-word;
  text-align: center;
  position: absolute;
  top: 10%;
}
</style>
