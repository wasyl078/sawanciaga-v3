<script setup lang="ts">
// ASCII Cat component - displays a centered ASCII art cat
</script>

<template>
  <div class="ascii-cat-container">
    <pre class="ascii-cat">
    /\__/\
    /'    '\
    === 0  0 ===
    \  --  /
    /        \
    /          \
    |           |
    \  ||  ||  /
            \_oo__oo_/#######o
    </pre>
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
  color: #ff8c42;
  text-align: center;
  white-space: pre;
  margin: 0;
  font-weight: bold;
  text-shadow: 0 0 10px rgba(255, 140, 66, 0.3);
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
