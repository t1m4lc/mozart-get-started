<script setup>
import { ref } from 'vue'

defineProps({
  number: { type: String, required: true },
  label: { type: String, required: true },
  subtitle: { type: String, default: '' },
  hasMore: { type: Boolean, default: false },
})

// Each step owns its own toggle — fully independent.
const showMore = ref(false)
</script>

<template>
  <section class="step">
    <div class="step-head">
      <span class="step-num">{{ number }}</span>
      <div class="step-title">
        <h3 class="step-label">{{ label }}</h3>
        <p v-if="subtitle" class="step-sub">{{ subtitle }}</p>
      </div>
    </div>

    <div class="step-action">
      <slot />
    </div>

    <template v-if="hasMore">
      <button class="more-toggle" type="button" @click="showMore = !showMore">
        {{ showMore ? '[ − more ]' : '[ + more ]' }}
      </button>
      <p v-if="showMore" class="more-body">
        <slot name="more" />
      </p>
    </template>
  </section>
</template>

<style scoped>
.step {
  border: 1px solid var(--border);
  border-radius: var(--radius);
  background: var(--surface);
  padding: 20px;
}

.step-head {
  display: flex;
  align-items: baseline;
  gap: 12px;
  margin-bottom: 14px;
}

.step-num {
  color: var(--violet);
  font-weight: 700;
  font-size: 13px;
  flex-shrink: 0;
}

.step-title {
  min-width: 0;
}

.step-label {
  margin: 0;
  font-size: 16px;
  font-weight: 700;
  letter-spacing: -0.01em;
}

.step-sub {
  margin: 4px 0 0;
  color: var(--muted);
  font-size: 13px;
  line-height: 1.5;
}

.more-toggle {
  margin-top: 14px;
  padding: 0;
  background: none;
  border: none;
  color: var(--muted);
  font-family: inherit;
  font-size: 13px;
  cursor: pointer;
  transition: color 0.15s ease;
}

.more-toggle:hover {
  color: var(--violet);
}

.more-body {
  margin: 10px 0 0;
  padding-left: 12px;
  border-left: 2px solid var(--violet);
  color: var(--muted);
  font-size: 13px;
  line-height: 1.6;
  font-style: italic;
}

@media (max-width: 420px) {
  .step {
    padding: 16px;
  }
}
</style>
