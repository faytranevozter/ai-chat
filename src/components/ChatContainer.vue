<script setup>
import { ref, onMounted, nextTick } from 'vue'
import { GoogleGenerativeAI } from '@google/generative-ai'
import ChatMessage from './ChatMessage.vue'
import ChatInput from './ChatInput.vue'

const messages = ref([])
const isLoading = ref(false)
const error = ref(null)
const chatContainerRef = ref(null)

let genAI = null
let model = null

// Initialize Gemini API
const initializeGemini = () => {
  const apiKey = import.meta.env.VITE_GEMINI_API_KEY
  
  if (!apiKey || apiKey === 'your_gemini_api_key_here') {
    error.value = 'Please set your Gemini API key in the .env file'
    return false
  }
  
  try {
    genAI = new GoogleGenerativeAI(apiKey)
    model = genAI.getGenerativeModel({ model: 'gemini-pro' })
    error.value = null
    return true
  } catch (err) {
    error.value = 'Failed to initialize Gemini API: ' + err.message
    return false
  }
}

// Scroll to bottom of chat
const scrollToBottom = () => {
  nextTick(() => {
    if (chatContainerRef.value) {
      chatContainerRef.value.scrollTop = chatContainerRef.value.scrollHeight
    }
  })
}

// Send message to AI
const sendMessage = async (message) => {
  if (!message.trim()) return
  
  // Add user message
  messages.value.push({
    id: Date.now(),
    role: 'user',
    content: message,
    timestamp: new Date()
  })
  
  scrollToBottom()
  
  if (!model && !initializeGemini()) {
    return
  }
  
  isLoading.value = true
  
  try {
    // Generate response
    const result = await model.generateContent(message)
    const response = await result.response
    const text = response.text()
    
    // Add AI response
    messages.value.push({
      id: Date.now() + 1,
      role: 'assistant',
      content: text,
      timestamp: new Date()
    })
    
    error.value = null
  } catch (err) {
    error.value = 'Failed to get response: ' + err.message
    messages.value.push({
      id: Date.now() + 1,
      role: 'assistant',
      content: 'Sorry, I encountered an error. Please try again.',
      timestamp: new Date(),
      isError: true
    })
  } finally {
    isLoading.value = false
    scrollToBottom()
  }
}

// Initialize on mount
onMounted(() => {
  initializeGemini()
  
  // Add welcome message
  if (!error.value) {
    messages.value.push({
      id: Date.now(),
      role: 'assistant',
      content: 'Hello! I\'m your AI assistant powered by Google Gemini. How can I help you today?',
      timestamp: new Date()
    })
  }
})
</script>

<template>
  <div class="chat-container">
    <div v-if="error" class="error-banner">
      {{ error }}
    </div>
    
    <div ref="chatContainerRef" class="messages-container">
      <ChatMessage
        v-for="msg in messages"
        :key="msg.id"
        :message="msg"
      />
      
      <div v-if="isLoading" class="loading-indicator">
        <div class="typing-dots">
          <span></span>
          <span></span>
          <span></span>
        </div>
      </div>
    </div>
    
    <ChatInput
      :disabled="isLoading || !!error"
      @send="sendMessage"
    />
  </div>
</template>

<style scoped>
.chat-container {
  display: flex;
  flex-direction: column;
  height: calc(100vh - 4rem);
  max-width: 900px;
  margin: 0 auto;
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

.error-banner {
  background: #fee;
  color: #c33;
  padding: 1rem;
  text-align: center;
  border-bottom: 1px solid #fcc;
}

.messages-container {
  flex: 1;
  overflow-y: auto;
  padding: 1.5rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.loading-indicator {
  display: flex;
  justify-content: flex-start;
  padding: 1rem;
}

.typing-dots {
  display: flex;
  gap: 0.5rem;
  padding: 1rem 1.5rem;
  background: #f0f0f0;
  border-radius: 18px;
}

.typing-dots span {
  width: 8px;
  height: 8px;
  background: #999;
  border-radius: 50%;
  animation: typing 1.4s infinite;
}

.typing-dots span:nth-child(2) {
  animation-delay: 0.2s;
}

.typing-dots span:nth-child(3) {
  animation-delay: 0.4s;
}

@keyframes typing {
  0%, 60%, 100% {
    opacity: 0.3;
    transform: translateY(0);
  }
  30% {
    opacity: 1;
    transform: translateY(-10px);
  }
}

@media (prefers-color-scheme: dark) {
  .chat-container {
    background: #1a1a1a;
  }
  
  .typing-dots {
    background: #2a2a2a;
  }
  
  .typing-dots span {
    background: #666;
  }
}
</style>
