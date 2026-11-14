<script setup lang="ts">
/**
 * Faux terminal. Chaque entrée de `lines` est une commande (cmd) avec une
 * sortie optionnelle (out). `step` révèle les commandes une à une.
 */
withDefaults(defineProps<{
  title?: string
  lines: { cmd?: string; out?: string; comment?: string }[]
  step?: number
}>(), { title: 'zsh', step: 99 })
</script>

<template>
  <div class="term">
    <div class="bar">
      <span class="dot" style="background:#ff5f57" />
      <span class="dot" style="background:#febc2e" />
      <span class="dot" style="background:#28c840" />
      <span class="name">{{ title }}</span>
    </div>
    <div class="body">
      <template v-for="(l, i) in lines" :key="i">
        <Transition name="fade-up">
          <div v-if="i < step" class="entry">
            <div v-if="l.comment" class="comment"># {{ l.comment }}</div>
            <div v-if="l.cmd" class="cmd"><span class="prompt">$</span> {{ l.cmd }}</div>
            <pre v-if="l.out" class="out">{{ l.out }}</pre>
          </div>
        </Transition>
      </template>
    </div>
  </div>
</template>

<style scoped>
.term {
  background: #0d1117;
  border: 1px solid #30363d;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 20px 50px -20px rgba(0, 0, 0, 0.7);
  font-family: 'Fira Code', ui-monospace, monospace;
  font-size: 0.78rem;
  text-align: left;
}
.bar {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 8px 12px;
  background: #161b22;
  border-bottom: 1px solid #30363d;
}
.dot { width: 11px; height: 11px; border-radius: 50%; }
.name { margin-left: 8px; opacity: 0.5; font-size: 0.7rem; }
.body { padding: 12px 14px; display: flex; flex-direction: column; gap: 8px; }
.prompt { color: #3fb950; font-weight: 700; margin-right: 4px; }
.cmd { color: #e6edf3; white-space: pre-wrap; overflow-wrap: anywhere; padding-left: 1.1em; text-indent: -1.1em; }
.comment { color: #8b949e; font-style: italic; }
.out {
  margin: 2px 0 0;
  padding: 0;
  background: transparent;
  color: #8b949e;
  white-space: pre-wrap;
  font-size: 0.72rem;
}
</style>
