<script setup lang="ts">
/**
 * Illustration de l'analogie du conteneur maritime : une boîte standard qui
 * embarque l'app + tout ce dont elle a besoin, et qui tourne partout.
 */
withDefaults(defineProps<{ step?: number }>(), { step: 99 })

const content = [
  { icon: '🧩', label: 'Ton code' },
  { icon: '⚙️', label: 'Runtime (Node, Java, Python…)' },
  { icon: '📚', label: 'Librairies & dépendances' },
  { icon: '🔧', label: 'Configuration' },
]
const targets = ['💻 Laptop', '🏢 Serveur', '☁️ Cloud', '🤖 CI/CD']
</script>

<template>
  <div class="wrap">
    <div class="box">
      <div class="ribs" />
      <div class="inner">
        <div class="label"><logos-docker-icon /> conteneur</div>
        <template v-for="(c, i) in content" :key="c.label">
          <Transition name="fade-up">
            <div v-if="step > i" class="item">
              <span>{{ c.icon }}</span>{{ c.label }}
            </div>
          </Transition>
        </template>
      </div>
    </div>

    <Transition name="fade-up">
      <div v-if="step >= content.length + 1" class="targets">
        <div class="arrow">tourne à l'identique sur ➜</div>
        <div v-for="t in targets" :key="t" class="target">{{ t }}</div>
      </div>
    </Transition>
  </div>
</template>

<style scoped>
.wrap { display: flex; align-items: center; gap: 36px; justify-content: center; }
.box {
  position: relative;
  width: 360px;
  border-radius: 10px;
  background: linear-gradient(180deg, #1d63ed, #1447b8);
  padding: 14px;
  box-shadow: 0 25px 60px -25px #1d63ed;
}
.ribs {
  position: absolute; inset: 0;
  border-radius: 10px;
  background: repeating-linear-gradient(90deg, rgba(255,255,255,0.07) 0 6px, transparent 6px 22px);
  pointer-events: none;
}
.inner {
  position: relative;
  background: rgba(3, 10, 30, 0.55);
  border-radius: 6px;
  padding: 14px;
  min-height: 220px;
  display: flex; flex-direction: column; gap: 8px;
}
.label { display: flex; gap: 6px; align-items: center; font-weight: 700; text-transform: uppercase; letter-spacing: 0.15em; font-size: 0.7rem; opacity: 0.8; margin-bottom: 4px; }
.item {
  background: rgba(255, 255, 255, 0.08);
  border: 1px solid rgba(255, 255, 255, 0.15);
  border-radius: 8px;
  padding: 7px 10px;
  font-size: 0.85rem;
  display: flex; gap: 10px;
}
.targets { display: flex; flex-direction: column; gap: 8px; }
.arrow { font-size: 0.8rem; opacity: 0.6; }
.target {
  padding: 8px 16px;
  border-radius: 10px;
  background: rgba(16, 185, 129, 0.1);
  border: 1px solid rgba(16, 185, 129, 0.4);
  font-weight: 600;
}
</style>
