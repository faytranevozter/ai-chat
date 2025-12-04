<script setup>
import { defineProps } from 'vue'

const props = defineProps({
  message: {
    type: Object,
    required: true
  }
})

const formatTime = (date) => {
  return new Date(date).toLocaleTimeString('en-US', {
    hour: '2-digit',
    minute: '2-digit'
  })
}
</script>

<template>
  <div 
    class="message" 
    :class="{ 
      'user-message': message.role === 'user',
      'assistant-message': message.role === 'assistant',
      'error-message': message.isError
    }"
  >
    <div class="message-content">
      <div class="message-text">{{ message.content }}</div>
      <div class="message-time">{{ formatTime(message.timestamp) }}</div>
    </div>
  </div>
</template>

<style scoped>
.message {
  display: flex;
  margin-bottom: 0.5rem;
}

.user-message {
  justify-content: flex-end;
}

.assistant-message {
  justify-content: flex-start;
}

.message-content {
  max-width: 70%;
  padding: 0.75rem 1rem;
  border-radius: 18px;
  word-wrap: break-word;
}

.user-message .message-content {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-bottom-right-radius: 4px;
}

.assistant-message .message-content {
  background: #f0f0f0;
  color: #333;
  border-bottom-left-radius: 4px;
}

.error-message .message-content {
  background: #fee;
  color: #c33;
  border: 1px solid #fcc;
}

.message-text {
  white-space: pre-wrap;
  line-height: 1.5;
}

.message-time {
  font-size: 0.75rem;
  opacity: 0.7;
  margin-top: 0.25rem;
  text-align: right;
}

@media (prefers-color-scheme: dark) {
  .assistant-message .message-content {
    background: #2a2a2a;
    color: #e0e0e0;
  }
}
</style>
