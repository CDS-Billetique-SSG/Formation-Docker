<script setup lang="ts">
withDefaults(defineProps<{
  icon: string
  title: string
  color?: string
  compact?: boolean
}>(), { color: 'var(--docker-light)', compact: false })
</script>

<template>
  <div class="card" :class="{ compact }" :style="{ '--c': color }">
    <div class="icon">{{ icon }}</div>
    <div class="title">{{ title }}</div>
    <div class="text"><slot /></div>
  </div>
</template>

<style scoped>
.card {
  height: 100%;
  padding: 16px;
  border-radius: 14px;
  background: var(--panel);
  border: 1px solid var(--panel-border);
  border-top: 3px solid var(--c);
  transition: transform 0.2s;
}
.card:hover { transform: translateY(-3px); }
.icon { font-size: 1.8rem; line-height: 1; }
.title { font-weight: 700; margin: 8px 0 4px; color: var(--c); }
.text { font-size: 0.8rem; opacity: 0.8; line-height: 1.4; }
.text :deep(p) { margin: 0; }
.compact { display: grid; grid-template-columns: auto 1fr; column-gap: 14px; padding: 12px 16px; align-items: center; }
.compact .icon { grid-row: span 2; font-size: 1.6rem; }
.compact .title { margin: 0; }
.text :deep(code) { font-size: 0.72rem; }
</style>
