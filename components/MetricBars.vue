<script setup lang="ts">
/**
 * Barres comparatives VM vs Conteneur. `vm` et `ctr` sont des valeurs
 * relatives (0-100) pour la largeur, `vmLabel`/`ctrLabel` le texte affiché.
 */
withDefaults(defineProps<{
  metrics: { name: string; vm: number; ctr: number; vmLabel: string; ctrLabel: string }[]
  step?: number
}>(), { step: 99 })
</script>

<template>
  <div class="metrics">
    <div class="legend">
      <span><i style="background:var(--vm)" /> VM</span>
      <span><i style="background:var(--docker-light)" /> Conteneur</span>
    </div>
    <template v-for="(m, i) in metrics" :key="m.name">
      <Transition name="fade-up">
        <div v-if="step > i" class="metric">
          <div class="name">{{ m.name }}</div>
          <div class="bars">
            <div class="bar-row">
              <div class="bar vm" :style="{ width: m.vm + '%' }" />
              <span>{{ m.vmLabel }}</span>
            </div>
            <div class="bar-row">
              <div class="bar ctr" :style="{ width: Math.max(m.ctr, 1.5) + '%' }" />
              <span>{{ m.ctrLabel }}</span>
            </div>
          </div>
        </div>
      </Transition>
    </template>
  </div>
</template>

<style scoped>
.metrics { display: flex; flex-direction: column; gap: 14px; }
.legend { display: flex; gap: 18px; font-size: 0.8rem; opacity: 0.8; justify-content: flex-end; }
.legend i { display: inline-block; width: 12px; height: 12px; border-radius: 3px; margin-right: 4px; vertical-align: middle; }
.metric { display: grid; grid-template-columns: 180px 1fr; align-items: center; gap: 14px; }
.name { font-weight: 600; font-size: 0.9rem; }
.bars { display: flex; flex-direction: column; gap: 4px; }
.bar-row { display: flex; align-items: center; gap: 10px; font-size: 0.75rem; }
.bar-row span { white-space: nowrap; opacity: 0.85; }
.bar { height: 16px; border-radius: 4px; transition: width 0.8s ease; max-width: 75%; }
.bar.vm { background: linear-gradient(90deg, #b45309, var(--vm)); }
.bar.ctr { background: linear-gradient(90deg, var(--docker), var(--docker-light)); }
</style>
