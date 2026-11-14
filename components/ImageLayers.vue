<script setup lang="ts">
/**
 * Associe chaque instruction d'un Dockerfile à une couche de l'image.
 * Les couches s'empilent du bas vers le haut au fil des clics.
 */
withDefaults(defineProps<{ step?: number }>(), { step: 99 })

const layers = [
  { instr: 'FROM node:22-alpine', label: 'Image de base (Alpine + Node)', color: '#475569', size: '~ 55 Mo' },
  { instr: 'WORKDIR /app', label: 'Dossier de travail', color: '#64748b', size: '0 o' },
  { instr: 'COPY package*.json ./', label: 'Manifestes npm', color: '#7c3aed', size: '~ 200 Ko' },
  { instr: 'RUN npm ci', label: 'node_modules', color: '#b45309', size: '~ 80 Mo' },
  { instr: 'COPY . .', label: 'Ton code source', color: '#059669', size: '~ 2 Mo' },
  { instr: 'CMD ["node", "server.js"]', label: 'Commande de démarrage', color: '#1d63ed', size: 'métadonnée' },
]
</script>

<template>
  <div class="grid grid-cols-2 gap-8 items-end">
    <div class="dockerfile">
      <div class="fname">📄 Dockerfile</div>
      <div
        v-for="(l, i) in layers"
        :key="l.instr"
        class="instr"
        :class="{ active: step === i + 1, done: step > i }"
      >
        <span class="ln">{{ i + 1 }}</span>
        <code>{{ l.instr }}</code>
      </div>
    </div>

    <div class="pile">
      <div class="writable" :class="{ show: step > layers.length }">
        ✏️ Couche du conteneur (lecture/écriture)
      </div>
      <TransitionGroup name="fade-up">
        <template v-for="(l, i) in [...layers].reverse()" :key="l.instr">
          <div v-if="step > layers.length - 1 - i" class="layer" :style="{ background: l.color }">
            <span>{{ l.label }}</span>
            <small>{{ l.size }}</small>
          </div>
        </template>
      </TransitionGroup>
      <div class="caption">📦 Image = empilement de couches en lecture seule</div>
    </div>
  </div>
</template>

<style scoped>
.dockerfile { background: #0d1117; border: 1px solid #30363d; border-radius: 12px; padding: 12px; }
.fname { font-size: 0.75rem; opacity: 0.6; margin-bottom: 8px; }
.instr { display: flex; gap: 10px; align-items: center; padding: 5px 8px; border-radius: 6px; opacity: 0.35; transition: all 0.3s; }
.instr.done { opacity: 0.85; }
.instr.active { opacity: 1; background: rgba(36, 150, 237, 0.18); box-shadow: inset 3px 0 0 var(--docker-light); }
.instr code { background: transparent !important; font-size: 0.8rem; }
.ln { font-size: 0.7rem; opacity: 0.5; width: 12px; }
.pile { display: flex; flex-direction: column; gap: 4px; min-height: 300px; justify-content: flex-end; }
.layer {
  display: flex; justify-content: space-between; align-items: center;
  padding: 8px 12px; border-radius: 6px; font-size: 0.82rem; font-weight: 600;
  box-shadow: 0 4px 10px -4px rgba(0,0,0,0.6);
}
.layer small { opacity: 0.75; font-weight: 400; }
.writable {
  padding: 8px 12px; border-radius: 6px; font-size: 0.8rem; text-align: center;
  border: 2px dashed var(--ok); color: #6ee7b7; opacity: 0; transition: opacity 0.4s;
}
.writable.show { opacity: 1; }
.caption { font-size: 0.72rem; opacity: 0.6; text-align: center; margin-top: 6px; }
</style>
