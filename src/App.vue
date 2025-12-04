<script setup>
import ChatContainer from './components/ChatContainer.vue'
</script>

<template>
  <div class="app">
    <header class="app-header">
      <h1>AI Chat with Gemini</h1>
    </header>
    <ChatContainer />
  </div>
</template>

<style scoped>
.app {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

.app-header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 1rem 2rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.app-header h1 {
  margin: 0;
  font-size: 1.5rem;
  font-weight: 600;
}
</style>
