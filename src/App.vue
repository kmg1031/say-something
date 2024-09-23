<template>
  <div id="app">
    <ChatComponent />
    <StickmanComponent />
  </div>
</template>

<script>
import ChatComponent from './components/ChatComponent.vue';
import StickmanComponent from './components/StickmanComponent.vue';

export default {
  name: 'App',
  components: {
    ChatComponent,
    StickmanComponent
  }
};
</script>

<style>
#app {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
  background-color: #f8f9fa;
}
</style>
