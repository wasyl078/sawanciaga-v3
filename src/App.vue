<script setup lang="ts">
import { ref } from 'vue'
import AsciiCat from './components/AsciiCat.vue'
import Terminal from './components/TerminalWindow.vue'
import CommandModal from './components/CommandModal.vue'

const isTyping = ref(false)
const isCommandModalOpen = ref(false)
const terminalRef = ref<InstanceType<typeof Terminal> | null>(null)

const handleTypingStart = () => {
  isTyping.value = true
}

const handleTypingStop = () => {
  isTyping.value = false
}

const handleOpenCommandModal = () => {
  isCommandModalOpen.value = true
}

const handleCloseCommandModal = () => {
  isCommandModalOpen.value = false
}

const handleSendCommand = (command: string) => {
  if (terminalRef.value) {
    terminalRef.value.processCommand(command)
  }
  isCommandModalOpen.value = false
}
</script>

<template>
  <div id="mesh_background"></div>
  <div class="app-layout">
    <div class="content-area">
      <AsciiCat :is-typing="isTyping" />
    </div>
    <div class="terminal-card">
      <Terminal
        ref="terminalRef"
        @typing-start="handleTypingStart"
        @typing-stop="handleTypingStop"
        @open-command-modal="handleOpenCommandModal"
      />
    </div>
  </div>
  <CommandModal
    :is-open="isCommandModalOpen"
    :is-typing="isTyping"
    @close="handleCloseCommandModal"
    @send-command="handleSendCommand"
  />
</template>

<style scoped>
@keyframes ani-mesh_background {
  0% {
    --s-start-0: 8%;
    --s-end-0: 56.590226783981535%;
    --c-0: hsla(217, 100%, 30%, 1);
    --x-0: 95%;
    --y-0: 75%;
    --s-start-1: 1%;
    --s-end-1: 62.39165434620126%;
    --x-1: 95%;
    --c-1: hsla(0, 0%, 0%, 1);
    --y-1: 15%;
    --s-start-2: 8.392121895570533%;
    --s-end-2: 48.52567687605722%;
    --c-2: hsla(279, 67%, 27%, 1);
    --x-2: 2%;
    --y-2: 86%;
    --s-start-3: 8.392121895570533%;
    --s-end-3: 48.52567687605722%;
    --x-3: 93%;
    --c-3: hsla(279, 67%, 27%, 1);
    --y-3: 94%;
    --x-4: 55%;
    --c-4: hsla(217, 100%, 39%, 1);
    --y-4: 101%;
    --s-start-4: 18%;
    --s-end-4: 49.94408311488525%;
    --s-start-5: 11.133917156423086%;
    --s-end-5: 51.76366794121349%;
    --c-5: hsla(217, 100%, 30%, 1);
    --x-5: 90%;
    --y-5: 47%;
    --s-start-6: 6%;
    --s-end-6: 36.01800730950539%;
    --y-6: 72%;
    --x-6: 46%;
    --c-6: hsla(217, 100%, 47%, 1);
  }

  50% {
    --s-start-0: 9;
    --s-end-0: 47.631590766043374%;
    --c-0: hsla(217, 100%, 30%, 1);
    --x-0: 25%;
    --y-0: 90%;
    --s-start-1: 9;
    --s-end-1: 73;
    --x-1: 3%;
    --c-1: hsla(0, 0%, 0%, 1);
    --y-1: 12%;
    --s-start-2: 9%;
    --s-end-2: 49.05074977970796%;
    --c-2: hsla(279, 67%, 27%, 1);
    --x-2: 75%;
    --y-2: 71%;
    --s-start-3: 9%;
    --s-end-3: 49.05074977970796%;
    --x-3: 51%;
    --c-3: hsla(279, 67%, 27%, 1);
    --y-3: 97%;
    --x-4: 28%;
    --c-4: hsla(217, 100%, 39%, 1);
    --y-4: 75%;
    --s-start-4: 9;
    --s-end-4: 49.07325835290392%;
    --s-start-5: 9;
    --s-end-5: 49.48618142677546%;
    --c-5: hsla(217, 100%, 30%, 1);
    --x-5: 42%;
    --y-5: 102%;
    --s-start-6: 9;
    --s-end-6: 47.075992875412794%;
    --y-6: 97%;
    --x-6: 42%;
    --c-6: hsla(217, 100%, 39%, 1);
  }

  100% {
    --s-start-1: 9;
    --s-end-1: 38.76465783663554%;
    --x-1: 54%;
    --c-1: hsla(0, 0%, 0%, 1);
    --y-1: 14%;
    --s-start-2: 10.200720718860145%;
    --s-end-2: 50.46456494288142%;
    --c-2: hsla(279, 67%, 27%, 1);
    --x-2: 98%;
    --y-2: 95%;
    --s-start-3: 10.200720718860145%;
    --s-end-3: 50.46456494288142%;
    --x-3: 25%;
    --c-3: hsla(279, 67%, 27%, 1);
    --y-3: 74%;
    --s-start-6: 9;
    --s-end-6: 47.075992875412794%;
    --y-6: 93%;
    --x-6: 90%;
    --c-6: hsla(217, 100%, 31%, 1);
  }
}

@property --s-start-0 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 8%;
}

@property --s-end-0 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 56.590226783981535%;
}

@property --c-0 {
  syntax: '<color>';
  inherits: false;
  initial-value: hsla(217, 100%, 30%, 1);
}

@property --x-0 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 95%;
}

@property --y-0 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 75%;
}

@property --s-start-1 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 1%;
}

@property --s-end-1 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 62.39165434620126%;
}

@property --x-1 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 95%;
}

@property --c-1 {
  syntax: '<color>';
  inherits: false;
  initial-value: hsla(0, 0%, 0%, 1);
}

@property --y-1 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 15%;
}

@property --s-start-2 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 8.392121895570533%;
}

@property --s-end-2 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 48.52567687605722%;
}

@property --c-2 {
  syntax: '<color>';
  inherits: false;
  initial-value: hsla(279, 67%, 27%, 1);
}

@property --x-2 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 2%;
}

@property --y-2 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 86%;
}

@property --s-start-3 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 8.392121895570533%;
}

@property --s-end-3 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 48.52567687605722%;
}

@property --x-3 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 93%;
}

@property --c-3 {
  syntax: '<color>';
  inherits: false;
  initial-value: hsla(279, 67%, 27%, 1);
}

@property --y-3 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 94%;
}

@property --x-4 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 55%;
}

@property --c-4 {
  syntax: '<color>';
  inherits: false;
  initial-value: hsla(217, 100%, 39%, 1);
}

@property --y-4 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 101%;
}

@property --s-start-4 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 18%;
}

@property --s-end-4 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 49.94408311488525%;
}

@property --s-start-5 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 11.133917156423086%;
}

@property --s-end-5 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 51.76366794121349%;
}

@property --c-5 {
  syntax: '<color>';
  inherits: false;
  initial-value: hsla(217, 100%, 30%, 1);
}

@property --x-5 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 90%;
}

@property --y-5 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 47%;
}

@property --s-start-6 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 6%;
}

@property --s-end-6 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 36.01800730950539%;
}

@property --y-6 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 72%;
}

@property --x-6 {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 46%;
}

@property --c-6 {
  syntax: '<color>';
  inherits: false;
  initial-value: hsla(217, 100%, 47%, 1);
}

#mesh_background {
  --c-0: hsla(217, 100%, 30%, 1);
  --x-0: 95%;
  --y-0: 75%;
  --x-1: 95%;
  --c-1: hsla(0, 0%, 0%, 1);
  --y-1: 15%;
  --c-2: hsla(279, 67%, 27%, 1);
  --x-2: 2%;
  --y-2: 86%;
  --x-3: 93%;
  --c-3: hsla(279, 67%, 27%, 1);
  --y-3: 94%;
  --x-4: 55%;
  --c-4: hsla(217, 100%, 39%, 1);
  --y-4: 101%;
  --c-5: hsla(217, 100%, 30%, 1);
  --x-5: 90%;
  --y-5: 47%;
  --y-6: 72%;
  --x-6: 46%;
  --c-6: hsla(217, 100%, 47%, 1);
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: hsla(262, 82%, 3%, 1);
  background-image:
    radial-gradient(
      circle at var(--x-0) var(--y-0),
      var(--c-0) var(--s-start-0),
      transparent var(--s-end-0)
    ),
    radial-gradient(
      circle at var(--x-1) var(--y-1),
      var(--c-1) var(--s-start-1),
      transparent var(--s-end-1)
    ),
    radial-gradient(
      circle at var(--x-2) var(--y-2),
      var(--c-2) var(--s-start-2),
      transparent var(--s-end-2)
    ),
    radial-gradient(
      circle at var(--x-3) var(--y-3),
      var(--c-3) var(--s-start-3),
      transparent var(--s-end-3)
    ),
    radial-gradient(
      circle at var(--x-4) var(--y-4),
      var(--c-4) var(--s-start-4),
      transparent var(--s-end-4)
    ),
    radial-gradient(
      circle at var(--x-5) var(--y-5),
      var(--c-5) var(--s-start-5),
      transparent var(--s-end-5)
    ),
    radial-gradient(
      circle at var(--x-6) var(--y-6),
      var(--c-6) var(--s-start-6),
      transparent var(--s-end-6)
    );
  animation: ani-mesh_background 120s linear infinite alternate;
  background-blend-mode: normal, normal, normal, normal, normal, normal, normal;
  will-change: transform, opacity;
  contain: paint;
  z-index: -1;
}

.app-layout {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: row;
  overflow: hidden;
  pointer-events: none;
  gap: 0;
  padding: 0;
}

.content-area {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 16px;
  pointer-events: auto;
  overflow: auto;
}

.terminal-card {
  flex: 1;
  backdrop-filter: blur(10px);
  background: rgba(0, 0, 0, 0.3);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 12px;
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
  pointer-events: auto;
  overflow: hidden;
  margin: 16px;
}

/* Mobile layout */
@media (max-width: 768px) {
  .app-layout {
    flex-direction: column;
  }

  .content-area {
    flex: 1;
    padding: 16px;
  }

  .terminal-card {
    flex: 1;
    margin: 16px;
    margin-bottom: 80px;
  }
}
</style>
