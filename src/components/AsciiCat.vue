<script setup lang="ts">
import { ref, computed, watch, onUnmounted } from 'vue'

interface Props {
  isTyping?: boolean
}

const props = withDefaults(defineProps<Props>(), {
  isTyping: false,
})

const mouthFrame = ref(0)
let animationInterval: ReturnType<typeof setInterval> | null = null

const startAnimation = () => {
  if (animationInterval) return
  animationInterval = setInterval(() => {
    mouthFrame.value = (mouthFrame.value + 1) % 2
  }, 200)
}

const stopAnimation = () => {
  if (animationInterval) {
    clearInterval(animationInterval)
    animationInterval = null
  }
  mouthFrame.value = 0
}

// Watch isTyping prop to start/stop animation
watch(
  () => props.isTyping,
  (newValue) => {
    if (newValue) {
      startAnimation()
    } else {
      stopAnimation()
    }
  },
)

onUnmounted(() => {
  stopAnimation()
})

const mouth = computed(() => {
  if (!props.isTyping) {
    return '--'
  }
  // Alternate between '--' and '0'
  return mouthFrame.value === 0 ? '--' : '0'
})

const catArt = computed(() => {
  return `    /\\__/\\
    /'    '\\
    === 0  0 ===
    \\  ${mouth.value}  /
    /        \\
    /          \\
    |           |
    \\  ||  ||  /
            \\_oo__oo_/#######o`
})
</script>

<template>
  <div class="ascii-cat-container">
    <pre class="ascii-cat">{{ catArt }}</pre>
  </div>
</template>

<style scoped>
.ascii-cat-container {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
}

.ascii-cat {
  font-family: 'Courier New', monospace;
  font-size: 32px;
  line-height: 1.2;
  color: #ffffff;
  text-align: center;
  white-space: pre;
  margin: 0;
  font-weight: bold;
  text-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
  user-select: none;
}

/* Responsive font size */
@media (max-width: 1024px) {
  .ascii-cat {
    font-size: 24px;
  }
}

@media (max-width: 768px) {
  .ascii-cat {
    font-size: 20px;
  }
}
</style>
