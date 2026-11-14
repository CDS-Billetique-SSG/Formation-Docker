<script setup lang="ts">
/**
 * Pile de couches comparant une machine virtuelle et des conteneurs.
 * `step` permet de révéler progressivement les couches (passer $clicks).
 */
import { computed } from 'vue'

const props = withDefaults(defineProps<{
  mode: 'vm' | 'container'
  step?: number
}>(), { step: 99 })

const apps = ['App A', 'App B', 'App C']

const isVm = computed(() => props.mode === 'vm')
const accent = computed(() => (isVm.value ? 'var(--vm)' : 'var(--docker-light)'))
</script>

<template>
  <div class="stack">
    <div class="title" :style="{ color: accent }">
      <span v-if="isVm">🖥️ Machines virtuelles</span>
      <span v-else>🐳 Conteneurs</span>
    </div>

    <!-- Unités applicatives (VM ou conteneur) -->
    <Transition name="fade-up">
      <div v-if="step >= 3" class="units">
        <div v-for="app in apps" :key="app" class="unit" :style="{ borderColor: accent }">
          <div class="layer app">{{ app }}</div>
          <div class="layer libs">Bins / Libs</div>
          <div v-if="isVm" class="layer guest">
            OS invité
            <small>~ 1-2 Go</small>
          </div>
        </div>
      </div>
    </Transition>

    <!-- Couche de virtualisation -->
    <Transition name="fade-up">
      <div v-if="step >= 2" class="layer engine" :class="isVm ? 'hyp' : 'dock'">
        <template v-if="isVm">Hyperviseur <small>(VirtualBox, VMware, Hyper-V…)</small></template>
        <template v-else><logos-docker-icon class="inline mr-1" /> Docker Engine <small>(runtime de conteneurs)</small></template>
      </div>
    </Transition>

    <Transition name="fade-up">
      <div v-if="step >= 1" class="layer host-os">
        {{ isVm ? 'OS hôte (optionnel selon l\'hyperviseur)' : 'OS hôte — noyau Linux partagé' }}
      </div>
    </Transition>

    <div class="layer hw">⚙️ Infrastructure (CPU · RAM · disque)</div>
  </div>
</template>

<style scoped>
.stack {
  display: flex;
  flex-direction: column;
  gap: 6px;
  min-height: 330px;
  justify-content: flex-end;
}
.title {
  font-weight: 700;
  font-size: 1.1rem;
  text-align: center;
  margin-bottom: auto;
}
.units {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 6px;
}
.unit {
  display: flex;
  flex-direction: column;
  gap: 4px;
  padding: 4px;
  border: 2px dashed;
  border-radius: 10px;
}
.layer {
  border-radius: 8px;
  padding: 6px 8px;
  text-align: center;
  font-size: 0.8rem;
  font-weight: 600;
  line-height: 1.2;
}
.layer small {
  display: block;
  font-weight: 400;
  opacity: 0.75;
  font-size: 0.65rem;
}
.app { background: #10b981; color: #052e1f; }
.libs { background: #334155; }
.guest { background: #b45309; color: #fff7ed; padding: 12px 6px; }
.hyp { background: #f59e0b; color: #1f1300; }
.dock { background: var(--docker); color: white; }
.host-os { background: #475569; }
.hw { background: #1e293b; border: 1px solid #334155; }
.engine small { display: inline; }
</style>
