<script setup lang="ts">
/**
 * Plusieurs versions d'un même outil qui tournent côte à côte sans conflit.
 */
withDefaults(defineProps<{ step?: number }>(), { step: 99 })

const versions = [
  { tag: 'node:18', port: 3018, color: '#64748b' },
  { tag: 'node:20', port: 3020, color: '#059669' },
  { tag: 'node:22', port: 3022, color: '#1d63ed' },
]
</script>

<template>
  <div class="host">
    <div class="h-title">💻 Une seule machine, zéro Node installé</div>
    <div class="row">
      <template v-for="(v, i) in versions" :key="v.tag">
        <Transition name="fade-up">
          <div v-if="step > i" class="ctr" :style="{ borderColor: v.color }">
            <div class="badge" :style="{ background: v.color }">{{ v.tag }}</div>
            <div class="ico">🐳</div>
            <span class="iso">isolé ✅</span>
          </div>
        </Transition>
      </template>
    </div>
    <div class="engine"><logos-docker-icon class="inline mr-1" /> Docker Engine</div>
  </div>
</template>

<style scoped>
.host { border: 2px solid #475569; border-radius: 16px; padding: 14px; background: rgba(71,85,105,0.1); }
.h-title { font-weight: 700; margin-bottom: 12px; font-size: 0.9rem; }
.row { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; min-height: 130px; }
.ctr {
  border: 2px solid; border-radius: 12px; padding: 12px;
  display: flex; flex-direction: column; align-items: center; gap: 6px;
  background: rgba(255,255,255,0.03);
}
.badge { padding: 2px 10px; border-radius: 999px; font-weight: 700; font-size: 0.8rem; color: white; }
.ico { font-size: 1.8rem; }
.iso { font-size: 0.72rem; opacity: 0.7; }
.engine { margin-top: 10px; text-align: center; padding: 6px; border-radius: 8px; background: var(--docker); font-weight: 600; font-size: 0.85rem; }
</style>
