<script setup lang="ts">
/**
 * Le cycle Dockerfile -> build -> Image -> run -> Conteneur(s), avec le registry.
 */
withDefaults(defineProps<{ step?: number }>(), { step: 99 })
</script>

<template>
  <div class="flow">
    <div class="row">
      <div class="node file">
        <div class="ico">📄</div>
        <div class="lbl">Dockerfile</div>
        <div class="sub">La recette</div>
      </div>

      <Transition name="fade-up">
        <div v-if="step >= 1" class="arrow"><code>docker build</code><span>➜</span></div>
      </Transition>

      <Transition name="fade-up">
        <div v-if="step >= 1" class="node image">
          <div class="ico">📦</div>
          <div class="lbl">Image</div>
          <div class="sub">Le moule (lecture seule)</div>
        </div>
      </Transition>

      <Transition name="fade-up">
        <div v-if="step >= 2" class="arrow"><code>docker run</code><span>➜</span></div>
      </Transition>

      <Transition name="fade-up">
        <div v-if="step >= 2" class="containers">
          <div v-for="n in 3" :key="n" class="node ctr">
            <div class="ico">🐳</div>
            <div class="lbl">Conteneur {{ n }}</div>
          </div>
          <div class="sub" style="text-align:center">Instances vivantes</div>
        </div>
      </Transition>
    </div>

    <Transition name="fade-up">
      <div v-if="step >= 3" class="registry-row">
        <div class="vline"><code>docker push</code> ⬆ ⬇ <code>docker pull</code></div>
        <div class="node registry">
          <div class="ico">☁️</div>
          <div class="lbl">Registry <small>(Docker Hub, GitLab, GHCR…)</small></div>
          <div class="sub">La bibliothèque d'images partagées</div>
        </div>
      </div>
    </Transition>
  </div>
</template>

<style scoped>
.flow { display: flex; flex-direction: column; gap: 14px; }
.row { display: flex; align-items: center; justify-content: center; gap: 10px; min-height: 230px; }
.node {
  border-radius: 14px;
  padding: 14px 16px;
  text-align: center;
  border: 1px solid var(--panel-border);
  background: var(--panel);
  min-width: 130px;
}
.ico { font-size: 1.9rem; line-height: 1; }
.lbl { font-weight: 700; margin-top: 6px; }
.lbl small { font-weight: 400; opacity: 0.6; font-size: 0.7rem; }
.sub { font-size: 0.7rem; opacity: 0.65; margin-top: 2px; }
.file { border-color: #a78bfa; box-shadow: 0 0 25px -10px #a78bfa; }
.image { border-color: var(--vm); box-shadow: 0 0 25px -10px var(--vm); }
.ctr { border-color: var(--docker-light); padding: 6px 12px; display: flex; align-items: center; gap: 8px; }
.ctr .ico { font-size: 1.2rem; }
.ctr .lbl { margin: 0; font-size: 0.85rem; }
.containers { display: flex; flex-direction: column; gap: 6px; }
.arrow { display: flex; flex-direction: column; align-items: center; font-size: 1.6rem; color: #64748b; }
.arrow code { font-size: 0.68rem; }
.registry-row { display: flex; flex-direction: column; align-items: center; gap: 6px; }
.vline { font-size: 0.8rem; color: #94a3b8; }
.vline code { font-size: 0.68rem; }
.registry { border-color: #22d3ee; box-shadow: 0 0 25px -10px #22d3ee; min-width: 360px; }
</style>
