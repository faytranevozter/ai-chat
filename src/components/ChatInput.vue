<script setup>
import { ref, defineEmits, defineProps } from 'vue'

const props = defineProps({
  disabled: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['send'])

const inputText = ref('')

const handleSubmit = () => {
  if (inputText.value.trim() && !props.disabled) {
    emit('send', inputText.value)
    inputText.value = ''
  }
}

const handleKeydown = (event) => {
  if (event.key === 'Enter' && !event.shiftKey) {
    event.preventDefault()
    handleSubmit()
  }
}
</script>

<template>
  <div class="chat-input-container">
    <div class="input-wrapper">
      <textarea
        v-model="inputText"
        :disabled="disabled"
        placeholder="Type your message..."
        rows="1"
        class="message-input"
        @keydown="handleKeydown"
      />
      <button
        :disabled="disabled || !inputText.trim()"
        class="send-button"
        @click="handleSubmit"
      >
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M22 2L11 13M22 2l-7 20-4-9-9-4 20-7z"/>
        </svg>
      </button>
    </div>
  </div>
</template>

<style scoped>
.chat-input-container {
  border-top: 1px solid #e0e0e0;
  padding: 1rem;
  background: white;
}

.input-wrapper {
  display: flex;
  gap: 0.5rem;
  align-items: flex-end;
}

.message-input {
  flex: 1;
  padding: 0.75rem 1rem;
  border: 1px solid #e0e0e0;
  border-radius: 24px;
  font-family: inherit;
  font-size: 1rem;
  resize: none;
  outline: none;
  max-height: 120px;
  overflow-y: auto;
  line-height: 1.5;
}

.message-input:focus {
  border-color: #667eea;
}

.message-input:disabled {
  background: #f5f5f5;
  cursor: not-allowed;
}

.send-button {
  padding: 0.75rem;
  border-radius: 50%;
  min-width: 48px;
  min-height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.send-button svg {
  transform: rotate(0deg);
  transition: transform 0.2s;
}

.send-button:hover:not(:disabled) svg {
  transform: rotate(45deg);
}

@media (prefers-color-scheme: dark) {
  .chat-input-container {
    border-top-color: #333;
    background: #1a1a1a;
  }
  
  .message-input {
    background: #2a2a2a;
    color: #e0e0e0;
    border-color: #333;
  }
  
  .message-input:focus {
    border-color: #667eea;
  }
  
  .message-input:disabled {
    background: #1a1a1a;
  }
}
</style>
