<script setup lang="ts">
/**
 * Schéma hôte <-> conteneur : mapping de port (-p) et montage de volume (-v).
 */
withDefaults(defineProps<{ step?: number }>(), { step: 99 })
</script>

<template>
  <div class="scene">
    <div class="host">
      <div class="h-title">💻 Ta machine (hôte)</div>

      <div class="h-items">
        <div class="pill browser">🛠️ ton app / DBeaver<br><code>localhost:5433</code></div>
        <div class="pill folder">📁 <code>./data</code></div>
      </div>

      <div class="ctr">
        <div class="c-title">🐳 conteneur <code>postgres</code></div>
        <div class="c-items">
          <div class="pill port">🔌 port <code>5432</code></div>
          <div class="pill folder">📁 <code>/var/lib/postgresql/data</code></div>
        </div>
        <div class="c-note">Isolé : son propre réseau, son propre système de fichiers</div>
      </div>

      <Transition name="fade-up">
        <div v-if="step >= 1" class="link port-link">
          <code>-p 5433:5432</code>
          <span>hôte ⟷ conteneur</span>
        </div>
      </Transition>
      <Transition name="fade-up">
        <div v-if="step >= 2" class="link vol-link">
          <code>-v ./data:/var/lib/postgresql/data</code>
          <span>les données survivent au conteneur 💾</span>
        </div>
      </Transition>
    </div>
  </div>
</template>

<style scoped>
.scene { display: flex; justify-content: center; }
.host {
  position: relative;
  isolation: isolate;
  width: 100%;
  border: 2px solid #475569;
  border-radius: 16px;
  padding: 16px 20px 20px;
  background: rgba(71, 85, 105, 0.1);
  display: grid;
  grid-template-columns: 1fr 1.1fr 1.5fr;
  grid-template-rows: auto 1fr;
  gap: 12px 18px;
}
.h-title { grid-column: 1 / -1; font-weight: 700; }
.h-items { display: flex; flex-direction: column; gap: 70px; justify-content: center; }
.ctr {
  grid-column: 3;
  grid-row: 2;
  border: 2px solid var(--docker-light);
  border-radius: 14px;
  background: rgba(29, 99, 237, 0.12);
  padding: 12px;
  box-shadow: 0 0 40px -15px var(--docker-light);
}
.c-title { font-weight: 700; margin-bottom: 10px; }
.c-items { display: flex; flex-direction: column; gap: 56px; }
.c-note { font-size: 0.7rem; opacity: 0.6; margin-top: 12px; }
.pill {
  padding: 8px 12px; border-radius: 10px; font-size: 0.8rem;
  background: rgba(255,255,255,0.06); border: 1px solid var(--panel-border);
}
.pill code { font-size: 0.72rem; }
.link {
  grid-column: 2;
  display: flex; flex-direction: column; align-items: center; justify-content: center;
  text-align: center; font-size: 0.72rem; position: relative;
  padding: 8px 4px;
}
.link::before {
  content: '';
  position: absolute; left: -14px; right: -14px; top: 50%;
  height: 3px; border-radius: 3px;
  z-index: -1;
}
.link code { font-size: 0.7rem; background: #0d1117 !important; padding: 3px 6px; border-radius: 6px; }
.link span { opacity: 0.75; margin-top: 4px; background: #0b1220; padding: 0 4px; }
.port-link { grid-row: 2; align-self: start; margin-top: 4px; }
.port-link::before { background: linear-gradient(90deg, #22d3ee, var(--docker-light)); }
.port-link code { border: 1px solid #22d3ee; }
.vol-link { grid-row: 2; align-self: end; margin-bottom: 30px; }
.vol-link::before { background: linear-gradient(90deg, #a78bfa, var(--ok)); }
.vol-link code { border: 1px solid #a78bfa; }
</style>
