<script setup>
import { ref, onBeforeUnmount } from 'vue'

const props = defineProps({
  code: { type: String, required: true },
})

const copied = ref(false)
let timer = null

async function copy() {
  try {
    await navigator.clipboard.writeText(props.code)
    copied.value = true
    clearTimeout(timer)
    timer = setTimeout(() => (copied.value = false), 1600)
  } catch (err) {
    console.error('Clipboard write failed:', err)
  }
}

onBeforeUnmount(() => clearTimeout(timer))
</script>

<template>
  <div class="copyblock">
    <div class="cb-head">
      <button
        class="copy-btn"
        type="button"
        :class="{ copied }"
        @click="copy"
      >
        {{ copied ? '[ copied ✓ ]' : '[ copy ]' }}
      </button>
    </div>
    <pre class="code"><code>{{ code }}</code></pre>
  </div>
</template>

<style scoped>
.copyblock {
  border: 1px solid var(--border);
  border-radius: 8px;
  overflow: hidden;
  background: var(--surface);
}

.cb-head {
  display: flex;
  justify-content: flex-end;
  padding: 6px 8px;
  background: var(--surface-2);
  border-bottom: 1px solid var(--border);
}

.copy-btn {
  padding: 2px 6px;
  background: none;
  border: none;
  font-family: inherit;
  font-size: 12px;
  color: var(--muted);
  cursor: pointer;
  transition: color 0.15s ease;
}

.copy-btn:hover,
.copy-btn.copied {
  color: var(--violet);
}

.code {
  margin: 0;
  padding: 14px;
  font-family: inherit;
  font-size: 13px;
  line-height: 1.6;
  color: var(--text);
  white-space: pre-wrap;
  word-break: break-word;
  overflow-wrap: anywhere;
}
</style>
