<script setup lang="ts">
/**
 * Le classique "ça marche sur ma machine" : trois environnements qui divergent.
 */
withDefaults(defineProps<{ step?: number }>(), { step: 99 })

const envs = [
  { who: '👩‍💻 Ma machine', os: 'macOS 15', node: 'Node 22', db: 'Postgres 16', ok: true },
  { who: '🧑‍💻 Collègue', os: 'Windows 11', node: 'Node 18', db: 'Postgres 13', ok: false },
  { who: '🏭 Production', os: 'Debian 12', node: 'Node 20', db: 'Postgres 15', ok: false },
]
</script>

<template>
  <div class="grid grid-cols-3 gap-5">
    <template v-for="(e, i) in envs" :key="e.who">
      <Transition name="fade-up">
        <div v-if="step > i" class="env" :class="e.ok ? 'ok' : 'ko'">
          <div class="who">{{ e.who }}</div>
          <div class="screen">
            <div class="line"><span>OS</span>{{ e.os }}</div>
            <div class="line"><span>Runtime</span>{{ e.node }}</div>
            <div class="line"><span>BDD</span>{{ e.db }}</div>
          </div>
          <div class="status">{{ e.ok ? '✅ Ça marche !' : '💥 Ça plante…' }}</div>
        </div>
      </Transition>
    </template>
  </div>
</template>

<style scoped>
.env {
  border-radius: 14px;
  padding: 16px;
  border: 1px solid;
  text-align: center;
}
.ok { border-color: rgba(16, 185, 129, 0.5); background: rgba(16, 185, 129, 0.06); }
.ko { border-color: rgba(239, 68, 68, 0.5); background: rgba(239, 68, 68, 0.06); }
.who { font-weight: 700; font-size: 1.05rem; margin-bottom: 10px; }
.screen {
  background: #0d1117;
  border: 6px solid #1f2937;
  border-radius: 8px;
  padding: 10px;
  font-family: ui-monospace, monospace;
  font-size: 0.78rem;
  display: flex; flex-direction: column; gap: 4px;
}
.line { display: flex; justify-content: space-between; }
.line span { opacity: 0.5; }
.status { margin-top: 12px; font-weight: 700; }
</style>
