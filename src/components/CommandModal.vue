<script setup lang="ts">
import { ref, nextTick, watch } from 'vue'

interface Props {
  isOpen: boolean
  isTyping: boolean
}

interface Emits {
  (e: 'send-command', command: string): void
  (e: 'close'): void
}

const props = defineProps<Props>()
const emit = defineEmits<Emits>()

const modalInputValue = ref('')

const openModal = () => {
  modalInputValue.value = ''
  // Focus modal input after DOM update
  nextTick(() => {
    const modalInput = document.querySelector('.command-modal-input') as HTMLInputElement
    if (modalInput) {
      modalInput.focus()
    }
  })
}

const closeModal = () => {
  emit('close')
  modalInputValue.value = ''
}

const handleModalKeydown = (event: KeyboardEvent) => {
  if (event.key === 'Enter' && modalInputValue.value.trim() && !props.isTyping) {
    event.preventDefault()
    sendCommand()
  } else if (event.key === 'Escape') {
    event.preventDefault()
    closeModal()
  }
}

const handleModalBackdropClick = (event: MouseEvent) => {
  if (event.target === event.currentTarget) {
    closeModal()
  }
}

const sendCommand = () => {
  if (modalInputValue.value.trim() && !props.isTyping) {
    const command = modalInputValue.value.trim()
    emit('send-command', command)
    closeModal()
  }
}

// Watch for isOpen changes to handle focus
watch(
  () => props.isOpen,
  (newVal) => {
    if (newVal) {
      openModal()
    }
  },
)
</script>

<template>
  <div v-if="isOpen" class="command-modal-backdrop" @click="handleModalBackdropClick">
    <!-- Modal window -->
    <div class="command-modal-window">
      <div class="command-modal-title">Ask Sawanka</div>
      <input
        v-model="modalInputValue"
        @keydown="handleModalKeydown"
        type="text"
        class="command-modal-input"
        placeholder="Type your command..."
        :disabled="isTyping"
      />
      <div class="command-modal-buttons">
        <button
          class="command-modal-button command-modal-button-cancel"
          @click="closeModal"
          :disabled="isTyping"
        >
          Cancel
        </button>
        <button
          class="command-modal-button command-modal-button-continue"
          @click="sendCommand"
          :disabled="isTyping || !modalInputValue.trim()"
        >
          Continue
        </button>
      </div>
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

.command-modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10000;
  animation: fadeIn 0.2s ease-in-out;
  pointer-events: auto;
}

.command-modal-window {
  background: rgba(20, 20, 40, 0.95);
  border: 2px solid rgba(255, 255, 255, 0.2);
  border-radius: 12px;
  padding: 24px;
  max-width: 300px;
  width: 90%;
  box-shadow:
    0 8px 32px 0 rgba(31, 38, 135, 0.5),
    inset 0 1px 1px rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(10px);
  animation: slideUp 0.3s ease-out;
}

@keyframes slideUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.command-modal-title {
  color: #ffffff;
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 16px;
  font-family: 'Courier New', Consolas, monospace;
}

.command-modal-input {
  width: 100%;
  padding: 12px;
  background-color: rgba(0, 0, 0, 0.3);
  border: 1px solid rgba(255, 255, 255, 0.2);
  color: #ffffff;
  font-family: 'Courier New', Consolas, monospace;
  font-size: 14px;
  border-radius: 6px;
  margin-bottom: 20px;
  outline: none;
  caret-color: #ffffff;
  transition: all 0.2s ease;
  box-sizing: border-box;
}

.command-modal-input:focus {
  background-color: rgba(0, 0, 0, 0.5);
  border-color: rgba(255, 255, 255, 0.4);
  box-shadow: inset 0 0 0 2px rgba(100, 200, 255, 0.2);
}

.command-modal-input::placeholder {
  color: rgba(255, 255, 255, 0.5);
}

.command-modal-input:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.command-modal-buttons {
  display: flex;
  gap: 12px;
  justify-content: space-between;
}

.command-modal-button {
  flex: 1;
  padding: 10px 16px;
  border: 1px solid rgba(255, 255, 255, 0.3);
  border-radius: 6px;
  color: #ffffff;
  font-family: 'Courier New', Consolas, monospace;
  font-size: 13px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.2s ease;
  outline: none;
}

.command-modal-button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.command-modal-button-cancel {
  background-color: rgba(255, 100, 100, 0.1);
}

.command-modal-button-cancel:hover:not(:disabled) {
  background-color: rgba(255, 100, 100, 0.3);
  border-color: rgba(255, 100, 100, 0.6);
}

.command-modal-button-cancel:active:not(:disabled) {
  transform: scale(0.98);
}

.command-modal-button-continue {
  background: linear-gradient(135deg, rgba(100, 200, 255, 0.2), rgba(150, 100, 255, 0.2));
  border-color: rgba(100, 200, 255, 0.6);
}

.command-modal-button-continue:hover:not(:disabled) {
  background: linear-gradient(135deg, rgba(100, 220, 255, 0.4), rgba(150, 120, 255, 0.4));
  border-color: rgba(100, 200, 255, 0.8);
  box-shadow: 0 0 15px rgba(100, 200, 255, 0.2);
}

.command-modal-button-continue:active:not(:disabled) {
  transform: scale(0.98);
}

/* Mobile full screen modal */
@media (max-width: 768px) {
  .command-modal-backdrop {
    align-items: center;
    justify-content: center;
  }

  .command-modal-window {
    width: 90%;
    max-width: 90%;
    height: auto;
    border-radius: 12px;
    display: flex;
    flex-direction: column;
    padding: 20px;
    overflow-y: auto;
    box-sizing: border-box;
    position: relative;
    top: auto;
    left: auto;
    margin: 0;
  }

  .command-modal-title {
    flex-shrink: 0;
    margin-bottom: 16px;
    font-size: 18px;
  }

  .command-modal-input {
    flex-shrink: 0;
    font-size: 14px;
    padding: 12px;
    margin-bottom: 16px;
  }

  .command-modal-buttons {
    margin-top: 0;
    flex-shrink: 0;
    padding-top: 0;
    gap: 12px;
  }

  .command-modal-button {
    padding: 10px 16px;
    font-size: 13px;
  }
}

@media (max-width: 480px) {
  .command-modal-window {
    padding: 16px;
  }

  .command-modal-title {
    font-size: 18px;
    margin-bottom: 16px;
  }

  .command-modal-input {
    padding: 12px;
    font-size: 14px;
    margin-bottom: 20px;
  }

  .command-modal-button {
    padding: 10px 16px;
    font-size: 13px;
  }
}

@media (max-width: 360px) {
  .command-modal-window {
    padding: 12px;
  }

  .command-modal-title {
    font-size: 16px;
    margin-bottom: 12px;
  }

  .command-modal-input {
    padding: 10px;
    font-size: 13px;
    margin-bottom: 16px;
  }

  .command-modal-button {
    padding: 8px 12px;
    font-size: 12px;
  }
}
</style>
