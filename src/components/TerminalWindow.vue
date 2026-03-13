<script setup lang="ts">
import { ref, nextTick, onMounted } from 'vue'

interface Message {
  id: number
  text: string
  isCommand?: boolean
  isLoading?: boolean
}

const emit = defineEmits<{
  'typing-start': []
  'typing-stop': []
  'open-command-modal': []
}>()

const messages = ref<Message[]>([
  {
    id: 1,
    text: 'Welcome to Sawanciaga...',
    isCommand: false,
  },
])

const inputValue = ref('')
const messagesContainer = ref<HTMLElement | null>(null)
const isTyping = ref(false)
let messageCounter = 1

const processCommand = async (command: string) => {
  if (command.trim() && !isTyping.value) {
    isTyping.value = true
    emit('typing-start')

    // Add user command
    messageCounter++
    messages.value.push({
      id: messageCounter,
      text: command,
      isCommand: true,
    })

    scrollToBottom()

    // Add loading indicator
    messageCounter++
    const loadingMessageId = messageCounter
    messages.value.push({
      id: loadingMessageId,
      text: '.',
      isLoading: true,
    })

    scrollToBottom()

    // Animate dots: 1 dot, 2 dots, 3 dots cycles
    let dotCount = 1
    let cycleCount = 0
    const dotInterval = setInterval(() => {
      dotCount++
      if (dotCount > 3) {
        dotCount = 1
        cycleCount++
      }

      const loadingMsg = messages.value.find((m) => m.id === loadingMessageId)
      if (loadingMsg) {
        loadingMsg.text = '.'.repeat(dotCount)
      }

      // 3 complete cycles (each cycle: 1, 2, 3 dots)
      if (cycleCount >= 2) {
        clearInterval(dotInterval)
        // Remove loading message
        messages.value = messages.value.filter((m) => m.id !== loadingMessageId)

        // Generate and type response
        const response = generateCatResponse()
        typeResponse(response)
      }
    }, 200)
  }
}

const scrollToBottom = async () => {
  await nextTick()
  if (messagesContainer.value) {
    messagesContainer.value.scrollTop = messagesContainer.value.scrollHeight
  }
}

const generateCatResponse = (): string => {
  const numMiaus = Math.floor(Math.random() * 9) + 2 // 2-10 miaus
  const miaus: string[] = []

  // Calculate how many should be modified (20-50% of total)
  const maxModified = Math.floor(numMiaus * (0.2 + Math.random() * 0.3))
  const numModified = Math.floor(Math.random() * (maxModified + 1))

  // Create array of indices to modify
  const indicesToModify = new Set<number>()
  while (indicesToModify.size < numModified) {
    indicesToModify.add(Math.floor(Math.random() * numMiaus))
  }

  // Generate miaus
  for (let i = 0; i < numMiaus; i++) {
    if (indicesToModify.has(i)) {
      // Generate modified miau
      const variant = Math.random()
      if (variant < 0.5) {
        // Add random number of 'a's
        const numAs = Math.floor(Math.random() * 5) + 2 // 2-5 extra a's
        miaus.push('mi' + 'a'.repeat(1 + numAs) + 'u')
      } else {
        // Add random number of 'u's
        const numUs = Math.floor(Math.random() * 5) + 2 // 2-5 extra u's
        miaus.push('mia' + 'u'.repeat(1 + numUs))
      }
    } else {
      // Regular miau
      miaus.push('miau')
    }
  }

  let message = miaus.join(' ')

  // 5% chance to end with :P
  if (Math.random() < 0.05) {
    message += ' :P'
  }

  return message
}

const typeResponse = async (responseText: string): Promise<void> => {
  isTyping.value = true
  emit('typing-start')
  scrollToBottom()

  messageCounter++
  const messageId = messageCounter

  messages.value.push({
    id: messageId,
    text: '',
    isCommand: false,
  })

  scrollToBottom()

  // Type character by character with 5ms delay
  for (let i = 0; i < responseText.length; i++) {
    await new Promise((resolve) => setTimeout(resolve, 5))
    const message = messages.value.find((m) => m.id === messageId)
    if (message) {
      message.text += responseText[i]
    }
    scrollToBottom()
  }

  isTyping.value = false
  emit('typing-stop')
}

const handleInputFocus = () => {
  // Scroll input into view to avoid mobile keyboard covering it
  const inputElement = document.querySelector('.terminal-input') as HTMLInputElement
  if (inputElement) {
    inputElement.scrollIntoView({ behavior: 'smooth', block: 'nearest' })
  }
}

const handleInput = (event: KeyboardEvent) => {
  if (event.key === 'Enter' && inputValue.value.trim() && !isTyping.value) {
    const command = inputValue.value.trim()
    inputValue.value = ''
    processCommand(command)
  }
}

onMounted(() => {
  scrollToBottom()
})

defineExpose({
  processCommand,
})
</script>

<template>
  <div class="terminal">
    <div class="messages-container" ref="messagesContainer">
      <div
        v-for="message in messages"
        :key="message.id"
        class="message"
        :class="{ 'fade-in': true }"
      >
        <span v-if="message.isCommand" class="prompt">> </span>
        <span class="message-text">{{ message.text }}</span>
      </div>
    </div>
    <!-- Desktop input section -->
    <div class="input-section desktop-input">
      <span class="prompt">></span>
      <input
        v-model="inputValue"
        @keydown="handleInput"
        @focus="handleInputFocus"
        type="text"
        class="terminal-input"
        placeholder="Ask Sawanka..."
        :disabled="isTyping"
      />
    </div>

    <!-- Mobile button section -->
    <div class="input-section mobile-button-section">
      <button class="ask-button" @click="() => emit('open-command-modal')" :disabled="isTyping">
        Ask Sawanka
      </button>
    </div>
  </div>
</template>

<style scoped>
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.terminal {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
  background-color: transparent;
  color: #ffffff;
  font-family: 'Courier New', Consolas, monospace;
  font-size: clamp(12px, 1.5vw, 16px);
  line-height: 1.6;
  box-shadow:
    inset 0 2px 8px rgba(0, 0, 0, 0.3),
    0 8px 16px rgba(0, 0, 0, 0.4);
  border-radius: 12px;
  overflow: hidden;
  user-select: none;
}

.messages-container {
  flex: 1;
  overflow-y: auto;
  overflow-x: hidden;
  padding: 12px;
  display: flex;
  flex-direction: column;
  gap: 2px;
  scrollbar-width: thin;
  scrollbar-color: rgba(255, 255, 255, 0) transparent;
  transition: scrollbar-color 0.3s ease;
  overscroll-behavior: contain;
}

.messages-container:hover {
  scrollbar-color: rgba(255, 255, 255, 0.4) rgba(0, 0, 0, 0.3);
}

.messages-container::-webkit-scrollbar {
  width: 6px;
}

.messages-container::-webkit-scrollbar-track {
  background: transparent;
}

.messages-container::-webkit-scrollbar-thumb {
  background-color: rgba(255, 255, 255, 0);
  border-radius: 3px;
  transition: background-color 0.3s ease;
}

.messages-container:hover::-webkit-scrollbar-thumb {
  background-color: rgba(255, 255, 255, 0.4);
}

.message {
  display: flex;
  gap: 0;
  word-break: break-word;
  animation: fadeIn 0.2s ease-in-out;
  touch-action: manipulation;
}

.prompt {
  color: #ffffff;
  flex-shrink: 0;
  margin-right: 6px;
}

.message-text {
  color: #ffffff;
  white-space: pre-wrap;
  word-wrap: break-word;
}

.input-section {
  display: flex;
  align-items: center;
  gap: 0;
  padding: 12px;
  flex-shrink: 0;
  position: sticky;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.1);
  min-height: 44px;
}

.terminal-input {
  flex: 1;
  background-color: transparent;
  border: none;
  color: #ffffff;
  font-family: 'Courier New', Consolas, monospace;
  font-size: clamp(12px, 1.5vw, 16px);
  outline: none;
  caret-color: #ffffff;
  transition:
    opacity 0.2s ease,
    box-shadow 0.2s ease;
  -webkit-tap-highlight-color: transparent;
}

.terminal-input::placeholder {
  color: #d8d8d8;
}

.terminal-input:focus {
  box-shadow: inset 0 0 0 2px rgba(255, 255, 255, 0.2);
  opacity: 1;
}

.terminal-input:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

/* Mobile button section */
.mobile-button-section {
  display: none;
  justify-content: center;
  align-items: center;
}

.ask-button {
  flex: 1;
  max-width: 200px;
  padding: 12px 24px;
  background: linear-gradient(135deg, rgba(100, 200, 255, 0.3), rgba(150, 100, 255, 0.3));
  border: 2px solid rgba(255, 255, 255, 0.3);
  color: #ffffff;
  font-family: 'Courier New', Consolas, monospace;
  font-size: 14px;
  font-weight: bold;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
}

.ask-button:hover:not(:disabled) {
  background: linear-gradient(135deg, rgba(100, 220, 255, 0.5), rgba(150, 120, 255, 0.5));
  border-color: rgba(255, 255, 255, 0.6);
  box-shadow: 0 0 20px rgba(100, 200, 255, 0.3);
}

.ask-button:active:not(:disabled) {
  transform: scale(0.95);
}

.ask-button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

@media (max-width: 768px) {
  .terminal {
    font-size: clamp(12px, 2vw, 14px);
    height: 100%;
  }

  .messages-container {
    padding: 12px;
    gap: 4px;
    line-height: 1.8;
  }

  .message {
    margin: 2px 0;
  }

  /* Hide desktop input, show mobile button */
  .desktop-input {
    display: none;
  }

  .mobile-button-section {
    display: flex;
  }

  .ask-button {
    font-size: 14px;
    padding: 12px 20px;
  }

  .prompt {
    margin-right: 8px;
  }
}

@media (max-width: 480px) {
  .terminal {
    font-size: 12px;
  }

  .messages-container {
    padding: 10px;
    gap: 3px;
    line-height: 1.8;
  }

  .mobile-button-section {
    padding: 10px;
    min-height: 44px;
  }

  .ask-button {
    font-size: 13px;
    padding: 10px 18px;
  }

  .modal-window {
    max-width: 280px;
    padding: 20px;
  }

  .prompt {
    margin-right: 6px;
  }
}

@media (max-width: 360px) {
  .terminal {
    font-size: 11px;
  }

  .messages-container {
    padding: 8px;
    gap: 2px;
  }

  .message {
    margin: 1px 0;
  }

  .prompt {
    display: none;
  }

  .mobile-button-section {
    padding: 8px;
    min-height: 40px;
    gap: 4px;
  }

  .ask-button {
    font-size: 12px;
    padding: 8px 16px;
    max-width: 160px;
  }

  .message-text {
    white-space: pre-wrap;
    word-break: break-word;
  }
}
</style>
